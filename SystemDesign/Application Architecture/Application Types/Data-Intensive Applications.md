A **data-intensive application** is a system where **data volume, data access patterns, and data reliability** are more critical than raw CPU computation.

📌 Examples:  
Search engines, recommendation systems, logging platforms, analytics pipelines, financial ledgers, social feeds, metrics systems

# 2️⃣ Core Characteristics of Data-Intensive Applications

- Data grows continuously (append-heavy systems)
- System behavior depends heavily on:
	- Read-heavy vs write-heavy
	- Random reads vs sequential scans
	- Low-latency reads vs batch analytics
- Data **must not be lost or corrupted**
- Strong guarantees around:
	- Durability
	- Ordering
	- Idempotency
	- Exactly-once / at-least-once semantics
- Storage and query performance must scale **horizontally**
- System explicitly chooses:
	- Strong consistency
	- Eventual consistency
	- Tunable consistency
- Data-intensive systems are **designed assuming failure**
- Latency percentiles

# 3️⃣ High-Level Design (HLD)
## A. Frontend (Client / Consumer Layer)
### Role
- Data visualization
- Query initiation
- Minimal business logic
### Characteristics
- **Stateless**
- Pagination[^1], filtering, aggregation handled carefully
- Often **read-optimized views**, not raw data
📌 Frontend rarely talks to DB directly — always via backend APIs
---
## B. Backend / Service Layer

This is the **brain** of a data-intensive system.

### Responsibilities
- Query orchestration
- Data validation & transformation
- Caching decisions[^2]
- Consistency enforcement[^3]
- Deduplication[^4]
- [[Rate limiting]]
---
### Read vs Write Path (Very Important)
#### 🔹 Write Path
- Often **asynchronous**
- Validates → enriches → persists → publishes events
- Designed for:
    - High throughput
    - Idempotency
    - Ordering (when required)
📌 Writes may be **append-only**
---
#### 🔹 Read Path
- Optimized for **low latency**
- Often served from:
    - Cache
    - Read replicas
    - Pre-computed views
- Avoids heavy joins at runtime
📌 Reads are usually **denormalized**
---
## C. Database Layer
### 1. Storage Model Choice

Depends on access pattern:

| Use Case              | Storage Type          |
| --------------------- | --------------------- |
| High write throughput | Log-structured stores |
| OLTP                  | Relational DB         |
| Analytics             | Columnar DB           |
| Time-series           | Time-series DB        |
| Search                | Inverted index        |

---
### 2. Data Modeling
- **Query-driven schema**
- Prefer:
    - Denormalization
    - Pre-aggregated tables[^5]
- Avoid cross-partition joins
📌 “Normalize for writes, denormalize for reads” is common
---
## D. Caching Strategy
### What to cache?
- Hot reads
- Aggregations
- Frequently accessed metadata
### Cache placement
- Application-level cache
- Distributed cache ([[Redis]]-like)
- [[CDN]] (for read-heavy public data)
### Cache consistency
- Write-through
- Write-behind
- Cache invalidation via events
📌 Cache is treated as **best-effort**, not source of truth
---
## E. Consistency Model
### Options
- **Strong consistency**
    - Financial systems
    - Inventory systems
- **Eventual consistency**
    - Feeds
    - Analytics
- **Tunable consistency**
    - Per-request guarantees
### Design Implication
- Read-after-write guarantees?
- Stale reads acceptable?
- Conflict resolution needed?
---
## F. Handling Duplicity
### Why duplicates happen?
- Retries
- Network failures
- At-least-once delivery
### Common Techniques
- Idempotency keys
- Deduplication tables
- Natural keys
- Versioning (timestamps / sequence numbers)
📌 Deduplication is **mandatory** in data-intensive systems
---
## G. Scalability Strategy
### Horizontal Scaling
- Stateless backend services
- Partitioned databases
- Sharded caches
---
### Data Partitioning
#### Partition Keys
- User ID
- Time
- Geography
- Hash-based keys
#### Goals
- Even data distribution
- Minimal cross-partition queries
- Predictable growth
📌 Bad partitioning = system collapse
---
## H. Fault Tolerance
### Techniques
- Replication
- Quorums[^6]
- Retry with backoff[^7]
- [[Circuit Breaker]]
- Graceful degradation
### Data Safety
- WAL / commit logs
- Snapshots
- Multi-AZ replication
---
## I. Performance Considerations
### Focus Areas
- Tail latency (p95/p99)
- Disk I/O
- Network hops
- Query complexity
### Optimizations
- Indexing
- Pre-aggregation
- Batching
- Async processing
📌 Performance ≠ speed only — **predictability matters**
---
## Final Interview Summary (Say This)

> A data-intensive application is designed around data volume, access patterns, and reliability.  
> Its HLD prioritizes scalable storage, optimized read/write paths, caching, explicit consistency trade-offs, partitioning, and fault tolerance — assuming failures are normal.

# 3️⃣ Low-Level Design (LLD)

# 1️⃣ **Append-Only / Log-Structured Pattern**
**Where:** Write path, ingestion layer, event storage  
**How:**
- All writes are **appended** (never updated in place)
- Background compaction merges and cleans data
**Why:**
- High write throughput
- Crash-safe and easy recovery
📌 Used in: event ingestion, metrics, audit logs
---
# 2️⃣ **Write-Ahead Log (WAL)**
**Where:** [[Database]] write path  
**How:**
- Persist intent to a log **before** mutating actual data
- Replay log on failure
**Why:**
- Guarantees durability
- Enables crash recovery
📌 Used in: relational DBs, distributed stores
---
# 3️⃣ **Idempotent Consumer / Idempotent Write**
**Where:** Ingestion APIs, message consumers  
**How:**
- Each request/event has a **unique idempotency key**
- Duplicate requests are safely ignored
**Why:**
- Handles retries & at-least-once delivery
📌 Mandatory for data pipelines and async systems
---
# 4️⃣ **CQRS (Command Query Responsibility Segregation)**
**Where:** Backend service layer  
**How:**
- Separate **write model** from **read model**
- Writes update source of truth
- Reads hit optimized projections or views
**Why:**
- Different scaling needs for reads vs writes
- Enables denormalized read models
📌 Common in read-heavy data systems
---
# 5️⃣ **Materialized View / Projection Pattern**
**Where:** Read layer, analytics layer  
**How:**
- Precompute and store query results
- Updated via async pipelines or streams
**Why:**
- Low-latency reads
- Avoid runtime joins and aggregations
📌 Used for dashboards, feeds, reports
---
# 6️⃣ **Sharding / Partitioning Logic**
**Where:** Data access layer  
**How:**
- Route reads/writes using a **partition key**
- Client or service decides shard
**Why:**
- Horizontal scalability
- Limits blast radius
📌 Poor partitioning = performance collapse
---
# 7️⃣ **Cache-Aside Pattern**
**Where:** Service → Cache → DB flow  
**How:**
- Read from cache
- On miss → DB → populate cache
- Writes update DB, then invalidate cache
**Why:**
- Simple and widely used
- Keeps cache non-authoritative
📌 Most common caching pattern
---
# 8️⃣ **Bulkhead Pattern**
**Where:** Service & resource isolation  
**How:**
- Separate thread pools / queues per workload
- Failure in one does not affect others
**Why:**
- Prevents cascading failures
- Protects critical paths
📌 Very important for multi-tenant data systems
---
# 9️⃣ **Retry + Circuit Breaker**
**Where:** Inter-service communication  
**How:**
- Retry with backoff for transient failures
- Open circuit on repeated failures
**Why:**
- Improves resilience
- Avoids overload during outages
📌 Always combined with idempotency
---
# 🔟 **Time-Windowing Pattern**
**Where:** Analytics & metrics systems  
**How:**
- Group data by fixed or sliding time windows
- Store aggregates per window
**Why:**
- Efficient queries on time-series data
📌 Used in monitoring, reporting systems
---
# 1️⃣1️⃣ **Versioning / Optimistic Locking**
**Where:** Concurrent writes to same data  
**How:**
- Maintain version or timestamp
- Reject conflicting updates
**Why:**
- Prevents lost updates
- Enables safe concurrent writes
📌 Critical in distributed writes
___

# 🎯 Interview Closing Line

> Data-intensive systems rely heavily on LLD patterns like append-only logs, idempotency, [[CQRS]], cache-aside, sharding, and materialized views to achieve scalability, correctness, and resilience under high data volume.

[^1]: - Pagination is typically implemented by **maintaining page state in React** (page number / cursor) and **fetching paginated data from the backend** using query params or cursors.
	- For large datasets, React uses **server-side pagination** (limit + offset or cursor-based) and renders only the current page to avoid loading all data at once.
[^2]: - Cache **hot, frequently read data** or expensive aggregations to reduce DB load and latency.
	- Choose strategy based on consistency needs: **read-through / write-through** for safer data, **TTL + async invalidation** for eventual consistency.
[^3]: - Enforced at the **write path**, not the cache: via **transactions, versioning, quorums, or ordering guarantees**.
	- Reads may be **stale**, but writes always follow the system’s chosen consistency model (strong / eventual / tunable).
[^4]: - Handled using **idempotency keys, unique constraints, or event IDs** to safely retry writes.
	- Commonly applied at **ingestion or write boundaries** to prevent duplicate data from retries or at-least-once delivery.
[^5]: - **What:** Tables that store **precomputed aggregates** (counts, sums, rollups) instead of calculating them at read time.
	- **Why:** Reduce **query latency and CPU load**, especially for read-heavy and analytics use cases.
	- **How they’re built:**
	    - Updated **asynchronously** via batch jobs or event streams
	    - Often **append + recompute** or **incrementally updated** on writes
	- **Consistency:**
	    - Usually **eventually consistent** with source data
	    - Accepts small staleness for faster reads
	- **When to use:**
	    - Dashboards, reports, metrics, feeds, analytics
	    - Avoid for highly volatile, transaction-critical data
[^6]: - **What:** A quorum is the **minimum number of replicas that must agree** on a read or write for it to be considered successful.
	- **How it works:**
	    - Write succeeds when **W replicas** acknowledge
	    - Read succeeds when **R replicas** respond
	    - If **R + W > N** (total replicas), strong consistency is achieved
	- **Why used:**
	    - Balances **consistency, availability, and latency**
	    - Tolerates node failures while avoiding stale reads
	- **Where used:**
	    - Distributed databases (e.g., Cassandra-like systems)
[^7]: - **What:** A failure-handling strategy where a request is **retried after increasing delays** instead of immediately retrying.
	- **Why:**
	    - Prevents **retry storms**
	    - Gives downstream systems time to recover
	- **How:**
	    - Use **exponential backoff**, often with **jitter** to avoid synchronized retries
	    - Limit retries and fail fast after a threshold