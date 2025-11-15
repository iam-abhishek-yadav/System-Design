## Non-Relational Databases

Non-relational databases (NoSQL) do not follow the traditional SQL relational model.  
They are designed for flexible schemas and `horizontal scaling (sharding) out of the box`.

```mermaid
graph TB
    NoSQL[NoSQL Databases] --> Doc[Document Databases]
    NoSQL --> KV[Key-Value Stores]
    NoSQL --> Graph[Graph Databases]
    
    Doc --> MongoDB[MongoDB]
    Doc --> Elasticsearch[Elasticsearch]
    
    KV --> Redis[Redis]
    KV --> DynamoDB[DynamoDB]
    KV --> Aerospike[Aerospike]
    
    Graph --> Neo4j[Neo4j]
    Graph --> Neptune[Amazon Neptune]
    Graph --> Dgraph[Dgraph]
    
    style NoSQL fill:#e1f5ff
    style Doc fill:#fff4e1
    style KV fill:#ffe1f5
    style Graph fill:#e1ffe1
```

The three main categories are:

### Document Databases
`Examples:` MongoDB, Elasticsearch

```mermaid
mindmap
  root((Document DBs))
    Characteristics
      JSON Documents
      Complex Queries
      Partial Updates
      Flexible Schema
      Semi-structured Data
    Use Cases
      User Profiles
      Content Management
      Product Catalogs
      Logging & Search
```

`Characteristics`
- Store data as JSON or JSON-like documents.
- Support complex queries (similar to relational DBs).
- Allow partial updates to documents.
- Flexible schema; closest NoSQL type to relational databases.
- Good for unstructured or semi-structured data.

`Use cases`
- User profiles  
- Content management  
- Product catalogs  
- Logging and search (Elasticsearch)

### Key-Value Stores
`Examples:` Redis, DynamoDB, Aerospike

```mermaid
graph LR
    A[Key-Value Store] --> B[GET]
    A --> C[PUT]
    A --> D[DELETE]
    
    B --> E[High Throughput]
    C --> E
    D --> E
    
    E --> F[Low Latency]
    E --> G[Heavy Sharding]
    
    style A fill:#ffe1f5
    style E fill:#fff4e1
    style F fill:#e1ffe1
    style G fill:#e1f5ff
```

`Characteristics`
- Extremely simple model: `GET`, `PUT`, `DELETE`.
- Designed for key-based access patterns.
- Do not support complex queries (no aggregations or joins).
- Can be heavily sharded and partitioned.
- High throughput and low latency.

`Use cases`
- Profile data  
- Auth/session data  
- Caching  
- Order lookups  
- Real-time counters and leaderboards


> Relational DBs and document DBs can also be used as key-value stores if needed.

### Graph Databases
`Examples:` Neo4j, Amazon Neptune, Dgraph

```mermaid
graph TB
    Node1[Node] -->|Edge| Node2[Node]
    Node2 -->|Relationship| Node3[Node]
    Node1 -->|Edge| Node3
    
    Node1 -.->|Optimized For| Algo[Graph Algorithms]
    Node2 -.->|Optimized For| Algo
    Node3 -.->|Optimized For| Algo
    
    Algo --> Use1[Social Networks]
    Algo --> Use2[Recommendations]
    Algo --> Use3[Fraud Detection]
    
    style Node1 fill:#e1ffe1
    style Node2 fill:#e1ffe1
    style Node3 fill:#e1ffe1
    style Algo fill:#fff4e1
```

`Characteristics`
- Represent data as `nodes`, `edges`, and `relationships`.
- Optimized for graph queries and graph algorithms.
- Extremely powerful for highly connected data.
- Ideal when relationships are first-class citizens.

`Use cases`
- Social networks  
- Recommendation engines  
- Fraud detection  
- Knowledge graphs  
- Network topology analysis
