Real-time application is one where **correctness depends on time**, not just result.

**Interpretation in system design terms**
- Latency matters more than throughput
- Delays = incorrect or useless result
- Event-driven, not request-driven

## 2️⃣ High-Level Architecture (Big Picture)

### Core idea

👉 **Push, don’t pull**  
👉 **Event-driven, async, state-aware**

### Logical layers

```
[ UI / Client ]
       ↓
[ Real-Time Gateway ]
       ↓
[ Event Processing Layer ]
       ↓
[ State Store + DB ]
       ↓
[ Stream / Message Bus ]

```

## 3️⃣ UI / Client Design (Frontend)
### Goals
- Low latency updates
- Server-push
- Minimal payloads
- Resilient to disconnects
### Key choices
#### 🔹 Communication[^1][^2]

| Option         | When                           |
| -------------- | ------------------------------ |
| WebSockets     | Bi-directional, true real-time |
| SSE            | Server → client only           |
| gRPC streaming | Native/mobile/internal         |
| Polling        | ❌ avoid                        |
👉 **Most real-time apps use [[WebSockets]]**
#### 🔹 UI Architecture
- Event-driven UI
- Immutable state updates
- Optimistic UI (don’t wait for server ack)
#### 🔹 Example
`User moves cursor → UI sends event → Server broadcasts → Other clients update UI`
### UI responsibilities
- Maintain connection
- Reconnect logic[^3]
- Client-side buffering
- Deduplication (idempotent events)[^4]
## 4️⃣ Backend Design (Core of Real-Time)
### 🔹 Entry Layer (Real-Time Gateway)
**Responsibilities**
- WebSocket lifecycle
- Auth at connection time
- Heartbeats / pings
- Connection → user mapping
**Tech**
- Netty
- Spring WebSocket
- Node.js (Socket.IO)
- Envoy / [[NGINX]] (L7 routing)
⚠️ **Stateless servers[^5] + external state**
### 🔹 Event Processing Layer
This is where real-time systems differ from CRUD apps.
**Characteristics**
- Async
- Non-blocking
- In-memory first
- Stateless processing[^6]
**Patterns**
- Actor model
- Event sourcing
- Stream processing
**Tech**
- [[Kafka]]
- Pulsar
- [[Redis]] Streams
- Akka
- Flink (if heavy logic)
---
### 🔹 Fan-out / Broadcast
Real-time apps are usually **1 → N**
**Strategies**
- In-memory pub/sub (Redis)
- [[Kafka]] consumer groups
- Region-based sharding
- User-room mapping
## 5️⃣ Database & State Design
### 🔹 Split the data types

| Data Type     | Storage           |
| ------------- | ----------------- |
| Live state    | In-memory (Redis) |
| Durable state | DB                |
| Event log     | Kafka             |
| Analytics     | OLAP              |

---
### 🔹 Why DB is NOT in hot path
❌ Writing to DB on every event = latency killer
✔ Pattern:
`Event → In-memory update → Broadcast → Async DB persistence`

---
### 🔹 Storage Choices
#### Hot state
- [[Redis]]
- [[Hazelcast]]
- Aerospike
#### Durable store

- [[PostgreSQL]] (if strong consistency)
- [[Cassandra]] (if massive scale)
- DynamoDB

#### Event log

- Kafka (most common)

---
## 6️⃣ End-to-End Flow Example (Stock Price Update)

`1. Market feed emits price update 2. Event pushed to Kafka 3. Stream processor updates Redis 4. WebSocket service broadcasts update 5. UI updates instantly 6. Async worker persists snapshot to DB`

Latency target: **< 50ms**

---
## 7️⃣ Consistency & Ordering
### Key problems
- Out-of-order events
- Duplicate messages
- Partial failures
### Solutions
- Event versioning[^7]
- Sequence numbers[^8]
- Idempotent handlers[^9]
- Per-key partitioning (Kafka)[^15]
---
## 8️⃣ Scalability Strategy
### Horizontal scaling
- Stateless WebSocket servers
- Sticky sessions (user → node)
- Redis for shared state
### Partitioning
- User-based
- Region-based
- Entity-based (stock symbol, game room)
---
## 9️⃣ Fault Tolerance

| Failure           | Handling           |
| ----------------- | ------------------ |
| Client disconnect | Reconnect + replay |
| Server crash      | State in Redis     |
| Event loss        | Kafka durability   |
| DB lag            | Async writes       |

---
## 🔟 Security
- Auth at WebSocket handshake[^10]
- Token refresh[^11]
- [[Rate limiting]][^12]
- Message validation[^13]
- Isolation per user/room[^14]
---
## 11️⃣ Common Mistakes (Interview Gold)
❌ Treating real-time app like REST
		Using synchronous request-response patterns in an event-driven system.
		- Introduces blocking calls
		- Increases latency
		- Breaks scalability
❌ DB in critical path  
		Performing database writes synchronously during real-time event flow.
		- DB becomes latency bottleneck
		- Slows down event propagation
		- Should be async
❌ No backpressure  
		Allowing producers to overwhelm consumers without flow control.
		- Memory usage grows uncontrollably
		- Leads to crashes under load
		- Causes cascading failures
❌ Broadcasting blindly  
		Sending events to all connected clients regardless of relevance.
		- Wastes bandwidth and CPU
		- Violates data isolation
		- Reduces system efficiency
❌ Ignoring reconnect semantics
		Failing to restore client state after reconnection.
		- Clients miss events
		- UI becomes inconsistent
		- State divergence occurs




---
## 12️⃣ Interview-Ready Summary (Say This)

> “A real-time application is designed as an event-driven system where UI uses [[WebSockets]] for server-push, backend processes events asynchronously using streams or pub/sub, hot state is kept in memory for low latency, durable persistence is async, and scalability is achieved via stateless services and partitioned event streams.”


## How Spring Boot Handles Real-Time
**Definition:** [[Spring Boot]] provides frameworks for building non-blocking, event-driven real-time systems.
- WebSocket for bidirectional communication
- [[WebFlux]] for reactive processing
- [[Kafka]] integration for event streaming
## Kafka vs Redis for Real-Time
### Kafka
**Definition:** Kafka is a durable distributed event-streaming platform.
- Guarantees ordering and replay
- Handles high throughput
- Used for event pipelines
### Redis
**Definition:** Redis is an in-memory data store optimized for ultra-low latency.
- Extremely fast pub/sub
- Used for hot state and fan-out
- Not designed for durable replay

___
# LLD Patterns for Real-Time Applications

---

## 1️⃣ [[Observer Pattern]]
**Definition:** Allows objects to subscribe and react automatically when state changes.
**Where used**
- UI updates on live data
- WebSocket message listeners
- In-memory state change propagation
**Why useful**
- Enables push-based updates
- Decouples event producers from consumers
---
## 2️⃣ [[Publish–Subscribe]] (Pub/Sub)
**Definition:** Producers publish events without knowing who consumes them.
**Where used**
- Kafka topics
- Redis Pub/Sub
- Event fan-out services
**Why useful**
- Supports 1→N real-time broadcasting
- Enables loose coupling and scalability
---
## 3️⃣ [[Reactor Pattern]]
**Definition:** Handles multiple asynchronous events using non-blocking event loops.
**Where used**
- WebSocket servers
- Netty
- Spring [[WebFlux]]
**Why useful**
- High concurrency with fewer threads
- Prevents thread blocking in real-time paths
---
## 5️⃣ [[Strategy Pattern]]
**Definition:** Allows selecting behavior at runtime.
**Where used**
- Message routing logic
- Event handling based on type/version
- Backpressure strategies
**Why useful**
- Avoids if-else explosion
- Makes real-time logic extensible
---
## 6️⃣ [[State Pattern]]
**Definition:** Changes object behavior based on its internal state.
**Where used**
- Connection lifecycle (CONNECTING, ACTIVE, CLOSED)
- Order lifecycle
- Game session state
**Why useful**
- Prevents invalid transitions
- Simplifies complex state machines
---
## 7️⃣ [[Circuit Breaker]]
**Definition:** Stops calls to failing components to prevent cascading failures.
**Where used**
- Downstream service calls
- External feeds
- Async persistence layer
**Why useful**
- Protects real-time latency
- Prevents thread and resource exhaustion
---
## 8️⃣ [[Bulkhead Pattern]]
**Definition:** Isolates resources so failures don’t spread.
**Where used**
- Separate thread pools for I/O and CPU
- Dedicated queues per stream
- User-level isolation
**Why useful**
- One slow consumer won’t block others
- Improves system resilience
---
## 9️⃣ [[Queue-Based Load Leveling]]
**Definition:** Smooths spikes by buffering work in queues.
**Where used**
- Incoming event ingestion
- Async DB writes
- Retry pipelines
**Why useful**
- Prevents overload during bursts
- Enables backpressure
---
## 🔟 [[Idempotent Consumer Pattern]]
**Definition:** Ensures processing the same event multiple times has no side effects.
**Where used**
- Kafka consumers
- WebSocket event replays
- Retry mechanisms
**Why useful**
- Safe retries
- Prevents duplicate updates
---
## 1️⃣2️⃣ [[CQRS]] (Command Query Responsibility Segregation)
**Definition:** Separates write and read models.
**Where used**
- High-write real-time systems
- Dashboards and projections
- Analytics views
**Why useful**
- Optimizes reads and writes independently
- Keeps real-time writes fast
---
## 1️⃣4️⃣ [[Chain of Responsibility]]
**Definition:** Passes a request through a chain of handlers.
**Where used**
- Message validation pipelines
- Security filters
- Event enrichment stages
**Why useful**
- Clean separation of concerns
- Easy to add or remove processing steps
---
## 1️⃣5️⃣ Scheduler / [[Time-Based Trigger Pattern]]
**Definition:** Executes tasks based on time or intervals.
**Where used**
- Heartbeats
- Timeout detection
- Session cleanup
**Why useful**
- Maintains connection health
- Detects dead clients quickly
---

# 🧠 How Interviewers Like to Hear This

> “Real-time systems heavily rely on Observer and Pub/Sub for event propagation, Reactor for non-blocking I/O, State and Strategy to manage complex runtime behavior, idempotent consumers for retries, and queue-based patterns for backpressure and load smoothing. These patterns collectively keep latency low and systems resilient.”

___
# Core Technologies & Traits in Real-Time Applications (Compact)
## 1️⃣ Event-Driven Architecture
**Definition:** System reacts to events instead of synchronous requests.
**Technologies**
- Kafka, Redis Pub/Sub, WebSocket events
**Why it matters**
- Enables push-based updates
- Eliminates request/response latency
- Scales naturally for 1→N communication
---
## 2️⃣ Persistent Streaming Connections
**Definition:** Long-lived connections for continuous data flow.
**Technologies**
- [[WebSockets]]
- SSE (read-only)
**Why it matters**
- Removes repeated HTTP overhead
- Enables instant server → client updates
- Reduces end-to-end latency
---
## 3️⃣ Non-Blocking I/O
**Definition:** Threads are not blocked waiting for I/O.
**Technologies**
- Netty
- Spring WebFlux
- Node.js event loop
**Why it matters**
- Handles thousands of concurrent connections
- Prevents thread exhaustion
- Keeps latency predictable under load
---
## 4️⃣ In-Memory Hot State
**Definition:** Frequently accessed state is stored in memory.
**Technologies**
- Redis
- Hazelcast
**Why it matters**
- Microsecond-level access
- Keeps DB out of critical path
- Enables fast fan-out
---
## 5️⃣ Durable Event Streaming
**Definition:** Events are stored and distributed reliably.
**Technologies**
- Kafka
- Pulsar
**Why it matters**
- Absorbs traffic spikes
- Guarantees ordering per key
- Enables replay after failure
---
## 6️⃣ Stateless Services
**Definition:** Servers do not keep user or session state locally.
**Technologies**
- [[Kubernetes]]
- Load balancers
- Redis for shared state
**Why it matters**
- Enables horizontal scaling
- Allows fast failover
- Avoids sticky sessions
---
## 7️⃣ Partitioning & Ordering
**Definition:** Work is split using entity-based keys.
**Technologies**
- Kafka partitions
- Key-based routing
**Why it matters**
- Preserves per-entity ordering
- Enables parallel processing
- Avoids coordination overhead
---
## 8️⃣ Idempotency & Deduplication
**Definition:** Reprocessing the same event does not change the outcome.
**Technologies**
- Event IDs
- Sequence numbers
**Why it matters**
- Safe retries
- Handles reconnects cleanly
- Prevents duplicate updates
---
## 9️⃣ Backpressure & Flow Control
**Definition:** System slows producers when consumers lag.
**Technologies**
- Kafka consumer lag
- Bounded queues
**Why it matters**
- Prevents memory overload
- Avoids cascading failures
- Stabilizes the system
---
# Interview-Ready One-Line Summary

> “Real-time applications use event-driven design, streaming connections, non-blocking I/O, in-memory state, durable event logs, stateless services, partitioned processing, and idempotent handling to achieve low latency, scalability, ordering, and fault tolerance.”


[^1]: SSE is a server-push mechanism where the server streams events to the client over HTTP.
	- Frontend uses `EventSource` to open a persistent HTTP connection
	- Backend keeps response open and flushes events continuously
	- Browser auto-reconnects with last received event ID

[^2]: Polling is a client-driven approach where the client repeatedly requests updates from the server.
	- Frontend calls API at fixed intervals
	- Long polling holds request until data is available
	- Creates unnecessary load and higher latency at scale

[^3]: - Reconnect logic ensures the client resumes real-time communication after connection loss.
	- Frontend detects disconnect via socket close or heartbeat failure
	- Reconnect happens using exponential backoff
	- Client sends last processed event ID to resume correctly**

[^4]: - Deduplication ensures the same event is processed only once even if delivered multiple times.
	- Each event carries a unique ID or sequence number
	- Client/server tracks last processed ID
	- Duplicate or older events are ignored

[^5]: Stateless servers do not store user or session state in local memory.
	- Any server instance can handle any request or connection
	- Enables horizontal scaling and failover
	- Shared state is stored in Redis or external systems

[^6]: Stateless processing handles each event independently without relying on in-memory state.
	- Event processing does not depend on previous events
	- Safe retries and parallel execution
	- Improves fault tolerance

[^7]: Event versioning allows event schemas to evolve without breaking consumers.
	- Each event includes a version field
	- Consumers branch logic based on version
	- Enables backward compatibility

[^8]: Sequence numbers enforce ordering of events for a given entity.
	- Events are processed only if sequence increases
	- Prevents out-of-order updates
	- Critical for consistency in real-time systems

[^9]: Idempotent handlers produce the same result even when the same event is processed multiple times.
	- Handler logic is retry-safe
	- Avoids incremental state updates
	- Prevents double writes

[^10]: Authentication is performed once when the WebSocket connection is established.
	- Client sends JWT during handshake
	- Server validates before accepting connection
	- Unauthorized clients are rejected immediately

[^11]: Token refresh maintains authentication for long-lived connections.
	- Access tokens expire while socket is open
	- Client refreshes token via REST API
	- Connection continues without user disruption

[^12]: Rate limiting restricts how many messages a client can send in a time window.
	- Applied per user or connection
	- Enforced at gateway or socket layer
	- Protects system from abuse

[^13]: Message validation ensures incoming real-time messages are safe and well-formed.
	- Validate schema, size, and permissions
	- Reject malformed payloads
	- Prevents security vulnerabilities

[^14]: Delivering events only to authorized users or logical groups.
	- Users subscribe to rooms or topics
	- Events are routed selectively
	- Prevents data leakage

[^15]: Kafka partitions messages based on a key to preserve ordering per entity.
	- Same key goes to same partition
	- Ordering is guaranteed
	- Enables parallel processing
