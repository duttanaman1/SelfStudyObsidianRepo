# 1 Application Types
## 1️⃣ [[Real-Time Applications]]
**Guarantee strict timing constraints**
**Characteristics**
- Deterministic latency (ms / µs)
- Deadline-driven
- Often embedded or control systems
**Examples**
- Air-traffic control
- Medical devices
- High-frequency trading (HFT)
- Robotics control systems
**Tech traits**
- RTOS
- Lock-free algorithms
- Predictable GC / no GC
---
## 2️⃣ [[Near Real-Time Applications]]
**Low latency, but not hard-deadline**
**Characteristics**
- Sub-second to few-seconds latency acceptable
- Event-driven
- Async processing
**Examples**
- Chat systems
- Live dashboards
- Fraud detection
- Notification systems
**Tech traits**
- [[Kafka]] / Pulsar
- [[WebSockets]]
- Reactive systems
---
## 3️⃣ [[Data-Intensive Applications]]
_(Classic from Martin Kleppmann)_
**Characteristics**
- Large data volume
- Focus on storage, replication, querying
- Throughput > latency
**Examples**
- Search engines
- Recommendation systems
- Logging platforms
- User activity tracking
**Tech traits**
- [[Cassandra]] / HBase
- [[Elasticsearch]]
- Data partitioning & compaction
---
## 4️⃣ [[Transaction-Heavy (OLTP) Applications]]
**Strong consistency + correctness**
**Characteristics**
- ACID transactions
- High concurrency
- Integrity critical
**Examples**
- Banking systems
- Order management
- Payment systems
- Inventory systems
**Tech traits**
- RDBMS ([[PostgreSQL]], Oracle)
- Distributed locks
- Idempotency, retries
---
## 5️⃣ [[Data Analytics (OLAP) Applications]]
**Read-heavy, compute-heavy**
**Characteristics**
- Complex queries
- Aggregations
- Batch or interactive analytics
**Examples**
- Business intelligence
- Reporting systems
- Data warehouses
**Tech traits**
- Snowflake / [[BigQuery]]
- Columnar storage
- Pre-aggregations
---
## 6️⃣ [[Stream Processing Applications]]
**Continuous data flow processing**
**Characteristics**
- Unbounded data
- Windowing
- Stateful processing
**Examples**
- Real-time metrics
- Clickstream analytics
- Fraud scoring pipelines
**Tech traits**
- [[Kafka]] Streams
- Flink
- Spark Streaming
---
## 7️⃣ High-Performance Computing (HPC) Applications
**Compute-bound systems**
**Characteristics**
- CPU/GPU intensive
- Parallel processing
- Floating-point heavy
**Examples**
- Weather simulations
- Scientific modeling
- Genomics
**Tech traits**
- MPI
- CUDA
- SIMD / vectorization
---
## 8️⃣ [[Distributed Systems]]
**Multiple nodes, failure-tolerant**
**Characteristics**
- Network partitions
- Partial failures
- Coordination complexity
**Examples**
- [[Microservices]] platforms
- Distributed databases
- Cloud control planes
**Tech traits**
- Consensus (Raft/Paxos)
- [[Service discovery]]
- [[Circuit Breaker]]
---
## 9️⃣[[ Event-Driven Applications]]
**React to events instead of requests**
**Characteristics**
- Loose coupling
- Async workflows
- Eventually consistent
**Examples**
- Order fulfillment pipelines
- Workflow engines
- Notification platforms
**Tech traits**
- Message brokers
- Saga pattern
- Event sourcing
---
## 🔟 Batch Processing Applications
**Offline, scheduled computation**
**Characteristics**
- Large datasets
- Long-running jobs
- Throughput-optimized
**Examples**
- Nightly reconciliation
- ETL jobs
- Payroll processing
**Tech traits**
- Spark
- Hadoop
- Airflow
---
## 1️⃣1️⃣ [[Interactive Applications]]
**Human-facing systems**
**Characteristics**
- UX latency sensitive
- Request/response driven
- Mixed read/write
**Examples**
- Web applications
- Mobile backends
- SaaS platforms
**Tech traits**
- REST / [[GraphQL]]
- Caching layers
- [[CDN]]
---
## 1️⃣2️⃣ Workflow-Oriented Applications
**Stateful business processes**
**Characteristics**
- Long-running flows
- Human + system steps
- Compensation logic
**Examples**
- Loan processing
- Insurance claims
- Approval systems
**Tech traits**
- BPM engines
- State machines
- Temporal / Cadence
---
## 1️⃣3️⃣ Embedded / Edge Applications
**Run close to hardware**
**Characteristics**
- Resource constrained
- Intermittent connectivity
- Deterministic behavior
**Examples**
- IoT devices
- Automotive systems
- Smart meters
**Tech traits**
- Lightweight runtimes
- Local caching
- OTA updates
---
## 1️⃣4️⃣ [[Platform Infrastructure Applications]]
**Used by other systems**
**Characteristics**
- Extreme reliability
- API-driven
- Multi-tenant
**Examples**
- API gateways
- Identity platforms
- Observability systems
**Tech traits**
- [[Rate limiting]]
- Multi-tenancy isolation
- Control planes
---
## 1️⃣5️⃣ AI / ML-Driven Applications
**Model-centric systems**
**Characteristics**
- Training vs inference split
- Probabilistic outputs
- Data drift concerns
**Examples**
- Recommendation engines
- LLM applications
- Vision systems
**Tech traits**
- Feature stores
- Model versioning
- GPU inference