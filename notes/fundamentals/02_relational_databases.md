# Relational Databases

Relational databases store data in `rows and columns` (tables).  
They ensure `data consistency, durability, and integrity` through `ACID properties` and `constraints`.

---

## 🧩 ACID Properties

```mermaid
graph LR
    A[ACID Properties] --> B[Atomicity]
    A --> C[Consistency]
    A --> D[Isolation]
    A --> E[Durability]
    
    B --> B1[All or Nothing]
    C --> C1[Valid State Transitions]
    C --> C2[Constraints]
    C --> C3[Cascades]
    C --> C4[Triggers]
    D --> D1[Transaction Visibility]
    E --> E1[Permanent Changes]
```

### 1. Atomicity
All statements within a transaction succeed or none do.  
→ No partial updates or half-completed operations.

```mermaid
sequenceDiagram
    participant T as Transaction
    participant DB as Database
    
    T->>DB: Begin Transaction
    T->>DB: Statement 1
    T->>DB: Statement 2
    T->>DB: Statement 3
    alt All succeed
        T->>DB: Commit
        DB-->>T: All changes saved
    else Any fails
        T->>DB: Rollback
        DB-->>T: All changes reverted
    end
```

### 2. Consistency
Data moves from one valid state to another.  
Ensured using:
- `Constraints` (e.g., foreign keys)
- `Cascades`
- `Triggers`

Example: A foreign key constraint ensures referential integrity.

### 3. Isolation
Determines how visible one transaction's changes are to others running simultaneously.

### 4. Durability
Once a transaction commits, the changes are permanent — they survive system outages or crashes.

```mermaid
sequenceDiagram
    participant T as Transaction
    participant DB as Database
    participant Disk as Persistent Storage
    
    T->>DB: Commit Transaction
    DB->>Disk: Write to disk
    Disk-->>DB: Confirmed
    DB-->>T: Committed
    Note over Disk: Data survives crashes
```
