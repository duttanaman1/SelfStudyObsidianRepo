Near real-time applications process and reflect data with a small, acceptable delay (milliseconds to seconds), not instantly but fast enough for business relevance.
## 1️⃣ Characteristics
**Core characteristics**
- **Bounded latency** – Updates appear within _seconds_ (not microseconds).
- **Event-driven** – Data flows via events, streams, or message queues.
- **Eventually consistent** – Strong consistency is usually traded for speed.
- **High throughput over instant response** – Batch-like streaming is common.
- **Asynchronous processing** – Requests don’t wait for full processing.
**What it’s NOT**
- ❌ Not hard real-time (no strict deadlines)
- ❌ Not offline/batch (results are continuously updated)
**Typical latency window**
- **100 ms → few seconds**
---
## 2️⃣ High-Level Design (HLD)
### 🔹 Architecture pattern
**Event-Driven + Stream Processing Architecture**
### 🔹 Typical flow
`Producers → Message Broker → Stream Processor → Storage → API / Dashboard`
### 🔹 Components
- **Data Producers**
    - [[Microservices]], IoT devices, logs, user actions
- **Message Broker**
    - Kafka / Pulsar / Kinesis
- **Stream Processing Layer**
    - Filters, aggregates, enriches events
- **Serving Layer**
    - Read-optimized DB / cache
- **Consumers**
    - Dashboards, alerts, downstream services
### 🔹 Key HLD decisions
- Partitioning strategy (by key, time, tenant)
- At-least-once vs exactly-once processing
- Backpressure handling
- Horizontal scalability
---
## 3️⃣ Low-Level Design (LLD)
### 🔹 Ingestion[^1]
- Producers publish **immutable events**
- Schema enforced (Avro / Protobuf / JSON Schema)
- Idempotent producers preferred
### 🔹 Processing
- Stateless processors for scaling
- Stateful processors for:
    - Windowed aggregations
    - Deduplication
- Time windows:
    - Tumbling
    - Sliding
    - Session windows
### 🔹 Storage
- **Hot data** → [[Redis]] / Elastic / Druid
- **Cold data** → S3 / HDFS / Data Lake
- Write-optimized + read-optimized separation
### 🔹 APIs
- Pull-based APIs for dashboards
- Push-based notifications for alerts
- Pagination + caching for freshness
### 🔹 Failure handling
- Retry with backoff
- Dead-letter queues[^2]
- Replay from offsets
---
## 4️⃣ Core Technologies
### 🔹 Messaging & Streaming
- **Apache Kafka** – industry standard
- Apache Pulsar
- AWS Kinesis
### 🔹 Stream Processing
- **Apache Flink** – low latency, stateful
- Kafka Streams – embedded processing
- Spark Structured Streaming – micro-batch
### 🔹 Storage
- [[Redis]] – fast reads
- [[Elasticsearch]] – near real-time search
- Apache Druid / ClickHouse – analytics
- [[Cassandra]] – high write throughput
### 🔹 APIs & Backend
- [[Spring Boot]] / [[WebFlux]]
- [[gRPC]] (internal consumers)
- REST (external dashboards)
### 🔹 Observability
- Lag monitoring (consumer offsets)
- Metrics ([[Micrometer]] + [[Prometheus]])
- Tracing (OpenTelemetry)
---
## 5️⃣ Core Traits (Interview Gold)
- **Latency-aware design** – seconds matter, not nanoseconds
- **Replayability** – ability to recompute from history
- **Loose coupling** – producers don’t know consumers
- **Scalable by partitioning** – scale by adding consumers
- **Graceful degradation** – stale data is acceptable briefly
---
## 6️⃣ Common Examples
- Live dashboards (analytics, ops)
- Fraud detection signals
- Stock price tickers
- Recommendation updates
- Log & metrics aggregation
- Order tracking systems

[^1]: **Ingestion** is the process of capturing, transporting, and loading data from various sources into a storage system or a processing pipeline. Think of it as the "entry gate" of your system.

[^2]: , a **Dead-Letter Queue (DLQ)** is a specialized buffer or storage area designed to hold messages that a system cannot process successfully. Instead of letting a "poison pill" message crash your workers or stall the entire pipeline, you move it to the side for later diagnosis.
