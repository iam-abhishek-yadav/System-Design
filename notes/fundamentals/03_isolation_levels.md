## Isolation Levels

Isolation levels decide how much one transaction knows about others that are running at the same time.

```mermaid
graph LR
    A[Isolation Levels] --> B[Read Uncommitted]
    A --> C[Read Committed]
    A --> D[Repeatable Read]
    A --> E[Serializable]
    
    B --> B1[Highest Visibility<br/>Dirty Reads Possible]
    C --> C1[Moderate Visibility<br/>No Dirty Reads]
    D --> D1[Low Visibility<br/>Consistent Reads]
    E --> E1[Lowest Visibility<br/>Serialized Execution]
    
    style B fill:#ffcccc
    style C fill:#ffffcc
    style D fill:#ccffcc
    style E fill:#ccccff
```

### Isolation Level Comparison

| Isolation Level     | Description | Visibility | Example Behavior |
|---------------------|-------------|------------|------------------|
| `Read Uncommitted` | Reads uncommitted values | Highest visibility | May see dirty reads |
| `Read Committed`   | Reads latest committed values | Moderate | Avoids dirty reads |
| `Repeatable Read`  | Ensures consistent reads in a single transaction | Low | Prevents non-repeatable reads |
| `Serializable`     | Every read locks rows | Lowest | Transactions run sequentially (like serialized) |

### Transaction Behavior Examples

```mermaid
sequenceDiagram
    participant T1 as Transaction 1
    participant T2 as Transaction 2
    participant DB as Database
    
    Note over T1,T2: Read Uncommitted
    T1->>DB: Write (uncommitted)
    T2->>DB: Read (sees uncommitted data)
    
    Note over T1,T2: Read Committed
    T1->>DB: Write
    T1->>DB: Commit
    T2->>DB: Read (sees committed data only)
    
    Note over T1,T2: Repeatable Read
    T2->>DB: Read value = 100
    T1->>DB: Update value = 200
    T1->>DB: Commit
    T2->>DB: Read value = 100 (same as before)
    
    Note over T1,T2: Serializable
    T2->>DB: Read (locks rows)
    T1->>DB: Write (waits for lock)
    T2->>DB: Commit (releases lock)
    T1->>DB: Write (proceeds)
```

### Important Notes

- Isolation levels impact `performance vs. consistency` trade-offs
- Storage engines (like InnoDB, RocksDB, etc.) can implement these differently — always `check the database documentation` for details

> Relational databases shine when you need `structured data with strong consistency` and `safe transactional behavior`.
