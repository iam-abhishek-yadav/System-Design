## Scaling a Database

### Overview
A database can be scaled in two ways:
- `Vertical scaling` — increase CPU, RAM, or disk on a single DB server.
- `Horizontal scaling` — add more DB servers using replication or sharding.

```mermaid
graph TB
    A[Database Scaling] --> B[Vertical Scaling]
    A --> C[Horizontal Scaling]
    
    B --> B1[Single Machine]
    B --> B2[Increase CPU/RAM/Disk]
    B --> B3[Physical Limit]
    
    C --> C1[Replication]
    C --> C2[Sharding]
    
    C1 --> C1A[Scale Reads]
    C2 --> C2A[Scale Writes & Storage]
    
    style B fill:#ffcccc
    style C fill:#ccffcc
```

### Vertical Scaling
Vertical scaling means increasing the resources of the existing database machine.

`Characteristics`
- Add more CPU, RAM, or disk.
- Often requires downtime.
- Helps handle more load.
- Has a physical upper limit — you eventually hit the maximum capacity of a single machine.

`Example capacity growth`
100 WPS → 200 WPS → 1000 WPS → 1500 WPS (limit reached)

When the limit is reached, you must move to horizontal scaling.

### Horizontal Scaling with Replication
Replication is used to scale `reads`.

```mermaid
graph TB
    subgraph "Replication Architecture"
        API[API Servers]
        Primary[Primary DB<br/>Writes Only]
        Replica1[Read Replica 1]
        Replica2[Read Replica 2]
        Replica3[Read Replica 3]
        
        API -->|Writes| Primary
        API -->|Reads| Replica1
        API -->|Reads| Replica2
        API -->|Reads| Replica3
        
        Primary -->|Replicate| Replica1
        Primary -->|Replicate| Replica2
        Primary -->|Replicate| Replica3
    end
    
    style Primary fill:#ffcccc
    style Replica1 fill:#ccffcc
    style Replica2 fill:#ccffcc
    style Replica3 fill:#ccffcc
```

`How it works`
- Primary (master) DB handles all writes.
- Read replicas handle read traffic.
- When read:write ≈ `90:10`, replicas offload heavy read load.
- API servers should know which database to connect to for reads and writes.

`Replication types`

```mermaid
sequenceDiagram
    participant API as API Server
    participant Primary as Primary DB
    participant Replica as Read Replica
    
    Note over API,Replica: Synchronous Replication
    API->>Primary: Write
    Primary->>Replica: Replicate (wait)
    Replica-->>Primary: Acknowledged
    Primary-->>API: Committed
    
    Note over API,Replica: Asynchronous Replication
    API->>Primary: Write
    Primary-->>API: Committed
    Primary->>Replica: Replicate (async)
    Replica-->>Primary: Acknowledged
```

- `Synchronous replication`  
  - Strong consistency  
  - Zero replication lag  
  - Slower writes  
- `Asynchronous replication`  
  - Eventual consistency  
  - Some replication lag  
  - Faster writes  

Replication ensures changes on the primary DB are copied to replicas to maintain consistency.

### Horizontal Scaling with Sharding
Sharding is used when one database node cannot handle all the data or write throughput.

```mermaid
graph TB
    subgraph "Sharding Architecture"
        API[API Servers]
        Router[Shard Router]
        Shard1[Shard 1<br/>Users 1-1000]
        Shard2[Shard 2<br/>Users 1001-2000]
        Shard3[Shard 3<br/>Users 2001-3000]
        
        API --> Router
        Router -->|Route by key| Shard1
        Router -->|Route by key| Shard2
        Router -->|Route by key| Shard3
    end
    
    style Router fill:#ffffcc
    style Shard1 fill:#ccffcc
    style Shard2 fill:#ccffcc
    style Shard3 fill:#ccffcc
```

`How it works`
- Data is split into multiple `exclusive, independent subsets`.
- Each subset is stored in a different shard.
- Writes for a particular record always go to its shard.
- Shards operate independently; there is no replication between shards.
- API servers must know which shard to route requests to.
- Some databases provide a routing proxy that handles this automatically.
- Each shard can have its own replica if needed.

### Sharding vs Partitioning
- `Sharding` — distributing data across multiple machines.  
- `Partitioning` — splitting a dataset into smaller subsets (may be within one machine or multiple).

### How Horizontal Partitioning Helps
Once a single machine hits its limit, you can add more machines and split the load.

Example:
- A system handling 1500 WPS on one machine  
- Split into two shards → ~750 WPS each  

### Ways to Partition Data

```mermaid
graph TB
    A[Partitioning Types] --> B[Horizontal Partitioning]
    A --> C[Vertical Partitioning]
    
    B --> B1[Split by Rows]
    B --> B2[Example: user_id ranges]
    B --> B3[Shard 1: Users 1-1000<br/>Shard 2: Users 1001-2000]
    
    C --> C1[Split by Tables/Columns]
    C --> C2[Example: User table in DB1<br/>Order table in DB2]
    C --> C3[Or: User basic info in DB1<br/>User profile in DB2]
    
    style B fill:#ccffcc
    style C fill:#ccccff
```

- `Horizontal partitioning` — separating rows of a table (e.g., user_id ranges).  
- `Vertical partitioning` — separating tables or splitting columns into different DBs.

### Advantages of Sharding
- Supports large read and write volumes.
- Increases total storage capacity.
- Improves availability (shards are independent).

### Disadvantages of Sharding
- Operationally complex.
- Cross-shard queries are expensive.
- Rebalancing and moving data across shards is difficult.

`Always minimize cross-shard queries.`
