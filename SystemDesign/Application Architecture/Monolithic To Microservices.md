## 1️⃣ Why migrate from Monolith to [[Microservices]]?

A **monolith** is a single deployable unit:

- One codebase
- One database (usually)
- One deployment pipeline
It works great **initially**, but over time:
### Pain points
- **Slow deployments** → one small change requires full redeploy
- **Tight coupling** → a bug in one module can crash everything
- **Scaling issues** → scale whole app instead of hot paths
- **Team velocity drops** → merge conflicts, ownership confusion
- **Tech lock-in** → hard to evolve stack per domain

### What microservices solve
- Independent deployability
- Independent scaling
- Clear domain ownership (team = service)
- Fault isolation
- Polyglot tech (when justified)

👉 Many monolith problems can be solved by:
- Modular monolith
- Better layering
- Async processing
- Caching

## 3️⃣ Migration strategies (MOST IMPORTANT)

### Strangler Fig Pattern (Most Common)
#### Idea
- Gradually “strangle” parts of the monolith
- Replace one feature at a time with a microservice
#### Flow
1. Monolith continues serving traffic
2. New [[microservice]] is built for a specific domain
3. Traffic is **routed** to the new service
4. Old monolith code is removed later

### Identify Service Boundaries (Domain-Driven Design (DDD)-based)
Split by **business capability**, not technical layers.
Each service owns
- Its logic
- Its data
- Its lifecycle
### Start with Low-Risk Services
Good first candidates:
- Notifications
- Reporting
- Auth (sometimes)
- File processing
- Async jobs
Avoid initially:
- Core transaction flow
- Highly coupled logic

## 4️⃣ [[Database]] & Data Migration (Hardest Part)

Each microservice owns its own database
### 4.1 Initial Phase (Transitional)
You may temporarily:
- Read from monolith DB
- Write via APIs
- Sync data using events
This is **acceptable short-term**
---
### 4.2 Patterns for Data Consistency
#### 🔹 [[Saga Pattern]]
Used when transactions span services
Example: Order → Payment → Inventory
- Each service performs local transaction
- Failures trigger **compensating actions**
No distributed transaction (no 2PC)
---
#### 🔹 Event-Driven Sync
- Monolith publishes events
- [[Microservices]] consume them
- Eventually consistent
## 5️⃣ Communication Between Services
### 5.1 Sync
- REST
- [[gRPC]]
- Simple but can cause cascading failures
### 5.2 Async (Recommended)
- [[Kafka]] / RabbitMQ
- Better resilience
- Decoupling

## 6️⃣ Infrastructure Changes Required

[[Microservices]] **force infra maturity**
#### Must-have
- [[API Gateway]]
- Service discovery[^1]
- Centralized logging[^2]
- Distributed tracing (Zipkin / Jaeger)
- Metrics & monitoring ([[Prometheus]] + [[Grafana]])
- CI/CD per service[^3]
- Containerization ([[Docker]] + [[Kubernetes]])

____

## 2️⃣ Failure Types & How We Handle Them (What Interviewers Want)

### 2.1 Service-Level Failures (Service Down / Slow)
#### Problem
- One microservice goes down
- Synchronous callers start failing
#### Solutions
- **[[Circuit Breaker]]s** (fail fast)
- **Timeouts** (never wait indefinitely)
- **Fallbacks** (graceful degradation)
Example:
- Show cached data
- Return partial response
> Key phrase: _“Prevent cascading failures.”_
---
### 2.2 Network Failures (Partial Connectivity)

#### Problem
- Network partitions
- High latency
- Intermittent timeouts
#### Solutions
- **Retries with [[exponential backoff]] + jitter**
- **Idempotent APIs** to make retries safe
❌ Never do infinite retries  
✅ Cap retry attempts

---
### 2.3 Data Consistency Failures During Migration
#### Problem
- Dual writes
- Partial rollout
- Inconsistent reads
#### Solutions
- **Shadow reads** (compare old vs new)
- **[[Feature flags]]** for quick rollback
- **Versioned APIs**
- **[[Transaction]]al outbox** (for Postgres)
---
### 2.4 Cross-Service Transaction Failures
#### Problem
- Order created
- Payment failed
- Inventory already reserved
#### Solution: [[Saga Pattern]]
Each step:
- Local transaction
- Emits event
Failure triggers:
- **Compensating transaction**
Example:
- Cancel order
- Refund payment
- Release inventory
This replaces distributed transactions.
---
### 2.5 Duplicate Requests & Messages
#### Problem
- Client retries
- Message redelivery
- At-least-once delivery
#### Solutions
- **Idempotency keys**
- **Unique constraints**
- **Conditional updates**
- **Deduplication tables**
---
### 2.6 [[Database]] Failures
#### During Migration
- Old monolith DB still in use
- New service DB added
#### Handling
- Read/write separation
- Health checks
- Failover-aware clients
- Graceful degradation
---
### 2.7 Deployment & Release Failures
#### Solutions
- [[Canary]] deployments
- [[Blue-Green]] deployments
- [[Fast rollback]]
- [[Feature flags]]

---

## 3️⃣ Migration-Specific Failure Controls (Very Important)

### 3.1 Dual-Write Safety

- Time-box dual writes
- Monitor mismatches
- Remove quickly
### 3.2 Backward Compatibility
- Never break existing consumers
- Support old API contracts
---
### 3.3 Rollback Strategy (Interviewers Love This)
If a microservice fails:
- Route traffic back to monolith
- Disable feature flag
- No data loss

> Always have a **rollback plan**.
---

## Step-by-step: How zero downtime is actually achieved

### 3.1 [[Strangler Fig Pattern]] (foundation)
- Monolith stays live
- [[Microservices]] are introduced gradually
### 3.2 [[API Gateway]] / Routing Layer
The gateway:
- Routes traffic to monolith or microservice
- Enables fast rollback
- Allows gradual traffic split
### 3.3 Backward-Compatible APIs (non-negotiable)
- Never break existing API contracts
- Add fields, never remove
- Version APIs when needed
### 3.4 [[Feature flags|Feature Flags]] (instant rollback)
Every migrated feature is guarded by a flag.
If something breaks:
- Turn off flag
- Traffic goes back to monolith
- No redeploy needed
### 3.5 Dual Writes (temporary, controlled)
During migration:
- Monolith writes to old DB
- Microservice writes to new DB
### 3.6 Shadow Reads (validation without risk)
- Read from monolith DB
- Read from microservice DB
- Compare responses
- Do not expose new data to users yet
### 3.7 [[Canary]] & [[Blue-Green]] Deployments
#### [[Canary]]
- Send small traffic (1–5%)
- Observe metrics
- Gradually increase
#### [[Blue-Green]]
- Deploy new version alongside old
- Switch traffic instantly
### 3.8 [[Database]] Migrations (often forgotten)
- Backward-compatible schema changes
- No blocking migrations
- Expand → migrate → contract pattern

[^1]: A mechanism that lets microservices dynamically locate each other without hard-coded addresses.
	Enables scalability, fault tolerance, and zero-downtime deployments as services scale up/down
	[[Kubernetes]] DNS / service registry ([[Consul]], Eureka) with client-side or server-side discovery.

[^2]: Aggregating logs from all services into a single, searchable system. Makes debugging and root-cause analysis possible in distributed systems. Log shipping via Fluentd/Logstash to [[Elasticsearch]], Loki, or Cloud logging platforms.

[^3]: Each microservice has its own independent build, test, and deployment pipeline. Allows teams to deploy services independently without blocking or risking others. [[Git]]-based pipelines ([[GitHub Actions]], [[Git]]Lab CI, [[Jenkins]]) triggering container builds and deployments to [[Kubernetes]].
