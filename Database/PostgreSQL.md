**PostgreSQL is an advanced, open-source, ACID-compliant relational database with MVCC, extensibility, and near-NoSQL flexibility (JSONB).**

---

## Why PostgreSQL Is Widely Used

- Strong **ACID guarantees**
- **MVCC** → high concurrency without read locks
- Rich SQL support (CTEs, window functions)
- Extensible (custom types, indexes, functions)
- Handles **OLTP + analytical queries** well

---

## Core Architecture (High-Level)

### 1. Process Model (Important Difference from MySQL)
- **One OS process per client connection**
- Background processes:
    - `checkpointer` Flushes dirty pages
    - `walwriter` Writes WAL (Write-Ahead Logging)
    - `autovacuum` Cleans dead tuples
    - `bgwriter` Writes buffers proactively

---
### 2. Storage Layout
- Tables are **heap files** (unordered)
- Index → separate structure
- WAL → sequential log

```
Client
   ↓ 
Postgres Backend
   ↓ 
Shared Buffers
   ↓ 
Disk (Heap + Index + WAL) 
```

---
## MVCC (Very Important)
### What It Solves
- Avoids read locks
- Enables high concurrency
### How It Works
- Each row has:
    - `xmin` (created by txn)
    - `xmax` (deleted by txn)
- Readers see a **snapshot*
👉 Reads don’t block writes  
👉 Writes don’t block reads

---

## Indexing in PostgreSQL

|Index|Use Case|
|---|---|
|B-Tree|Default, range queries|
|Hash|Equality only|
|GIN|JSONB, arrays|
|GiST|Geospatial|
|BRIN|Large, ordered tables|

`CREATE INDEX idx_user_email ON users(email);`

---

## JSON & Semi-Structured Data

`CREATE TABLE events (   id SERIAL,   payload JSONB );  SELECT payload->>'type' FROM events WHERE payload @> '{"source":"mobile"}';`

👉 Combines SQL + NoSQL patterns

---

## Transactions & Isolation Levels

|Level|Behavior|
|---|---|
|Read Committed|Default|
|Repeatable Read|Snapshot isolation|
|Serializable|Strictest (uses SSI)|

`BEGIN; SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;`

---

## Replication & High Availability

### 1. Streaming Replication
- Primary → Standby
- Physical replication
- Read replicas
### 2. Logical Replication
- Table-level
- CDC, migrations
---
## Partitioning

`CREATE TABLE orders (   id INT,   created_at DATE ) PARTITION BY RANGE (created_at);`

- Improves performance
- Reduces index size
- Works well with time-series data
---

## Vacuum & Autovacuum (Critical Topic)
### Why Needed
- MVCC creates dead tuples
- Vacuum cleans them
`VACUUM ANALYZE;`

Autovacuum prevents:

- Table bloat
- Transaction ID wraparound
---

## Performance Optimization (Practical)

### 1. Use EXPLAIN

`EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'x';`

### 2. Index Strategically
- Composite
- Partial
- Covering
### 3. Connection Pooling
- PgBouncer
- PgPool
---

## PostgreSQL vs MySQL (Quick)

|Feature|PostgreSQL|MySQL|
|---|---|---|
|MVCC|✅|Limited|
|JSON|JSONB (indexed)|Basic|
|Index Types|Many|Fewer|
|Standards|Strong|Weaker|
|Use Case|Complex systems|Simple OLTP|

---

## Real-World Use Cases
- Financial systems
- SaaS backends
- Analytics
- Event-driven systems
- [[Microservices]] data store