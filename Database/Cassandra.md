
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
[[Cassandra]] vs [[Database]]
### Cassandra excels when:

- Write-heavy workloads
- Massive data volume (TB–PB)
- Global, multi-region systems
- Always-on availability
- Predictable latency
### [[Database]] excels when:

- Strong consistency is mandatory
- Complex queries & joins
- Transactions across multiple entities
- Reporting & analytics
- Business logic at DB level
## Performance & Consistency Insight (interview favorite)

- **[[Cassandra]]**:
    - Optimized for **availability & scale**
    - Strong consistency only for specific operations (LWT)
- **[[Database]]**:
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


### **Tombstone[^2]
A **tombstone** in Cassandra is a **marker written for deleted data**. Cassandra doesn’t delete data immediately—it marks it as deleted and removes it later during **compaction**.

### **Lifecycle of a tombstone**

1. Delete request → tombstone written
2. Tombstone lives for `gc_grace_seconds` (default **10 days**)
3. Compaction removes it **only after all replicas have seen it**

---

### **Why tombstones are dangerous**

- Tombstones are **read during queries**
- Too many → **read latency spikes**
- Can cause:
    `TombstoneOverwhelmingException`

Especially harmful when:
- Large partitions
- Range scans
- `ALLOW FILTERING`
- TTL on frequently queried data

---

### **Golden rules to avoid tombstone issues**

- Avoid mass deletes
- Avoid TTL on hot data
- Keep partitions **bounded**
- Query using **exact partition key**
- Monitor tombstone metrics (`sstable tombstones scanned`)

___
## Cassandra Write Process

1. Client sends the request to coordinator node.
2. Coordinator node determines the target replica based on the partition key
3. Coordinator sends the write to replicas
4. Each replicas perform following actions:
	1. First the write is appended to the commit log (a write ahead log) on disk ensuring durability
	2. Write is applied to an in-memory structure called Memtable, a write optimized sorted hashmap.
	3. Send back ACK to Coordinator node once both write to commit log and memtable is successful.
5. Coordinator node recieves ACK and determine the success based on the consistency level (QUIRUM or ONE)

![[CAP Theorem#Example Cassandra (Tunable CAP)]]














[^1]: A distributed algorithm that ensures multiple nodes agree on a single value, even if some nodes fail or messages arrive out of order.

[^2]: Cassandra handles deletes using tombstones to ensure replica consistency in a distributed system. Since Cassandra is optimized for writes and sequential disk access, data is not removed immediately but cleaned up during compaction.  Data modeling in Cassandra is query-driven—tables are designed around access patterns, not normalization. Efficient queries must always specify the partition key and optionally clustering keys; otherwise, performance degrades due to partition scans and tombstone reads.
