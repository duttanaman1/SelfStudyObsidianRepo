
**Apache Cassandra is a distributed, masterless, peer-to-peer database** where all nodes are equal and data is partitioned using **consistent hashing** across the cluster.  
It provides **high availability and fault tolerance** through **replication, gossip protocol, and tunable consistency** (no single point of failure).

**LWT (Lightweight Transactions) in Cassandra provide linearizable consistency** by using a **Paxos-based consensus protocol**[^1], ensuring that conditional updates (IF conditions) are executed **atomically across replicas**.
- Used for **compare-and-set** operations (`IF NOT EXISTS`, `IF col = value`)
- Guarantees **strong consistency**, not high throughput
- Slower than normal writes due to **multiple round trips**
- Common for **uniqueness constraints, idempotency, and PII integrity**

Benefits of using Cassandra
* Masterless, Peer-to-Peer Architecture: No single point of failure
* Linear Horizontal Scalability: Scales without downtime
* High Availability & Fault Tolerance: Designed for 24×7 systems
* Tunable Consistency: Flexibility based on use case
* High Write Throughput: Excellent for write-heavy workloads
* **Multi-Data-Center Support**: Global applications & DR
* Schema Flexibility: Optimized for query-driven design
* Operational Stability at Scale: No sharding logic in app, Lower operational complexity at large scale
* 
[[Cassandra]] vs [[PostgreSQL]]
### Cassandra excels when:

- Write-heavy workloads
- Massive data volume (TB–PB)
- Global, multi-region systems
- Always-on availability
- Predictable latency
### [[PostgreSQL]] excels when:

- Strong consistency is mandatory
- Complex queries & joins
- Transactions across multiple entities
- Reporting & analytics
- Business logic at DB level
## Performance & Consistency Insight (interview favorite)

- **[[Cassandra]]**:
    - Optimized for **availability & scale**
    - Strong consistency only for specific operations (LWT)
- **[[PostgreSQL]]**:
    - Optimized for **correctness & relational integrity**
    - Consistency is default, not optional

Why migrate from Cassandra 4.x → 5.x
1. Performance Improvements
2. Improved **Paxos[^1] / LWT stability**
3. Fewer edge-case inconsistencies during node failures
4. Fewer GC and heap pressure issues
5. Easier performance tuning
6. Modernized Internal Architecture
7. Improved encryption handling

[^1]: A distributed algorithm that ensures multiple nodes agree on a single value, even if some nodes fail or messages arrive out of order.
