To reduce operational complexity and regain development velocity without losing logical separation.
A modular monolith is a single deployable application with **strict internal module boundaries**, high cohesion, and no runtime inter-module coupling.[^1]


## What problems were faced in Microservices?
### 1️⃣ Operational Complexity
- 20–50 services → CI/CD, monitoring, alerts, configs
- Service discovery, API gateway, retries, timeouts, circuit breakers everywhere
- Debugging requires tracing across services
> “A simple bug fix required touching 3 repos and redeploying 5 services.”
### 2️⃣ Data Consistency Pain
- Each service owns DB → **distributed transactions**
- Heavy reliance on:
    - Eventual consistency
    - Saga patterns
    - Compensating transactions
Common failures:
- Partial failures
- Orphan records
- Complex recovery logic
### 3️⃣ Performance & Latency
- Network calls instead of method calls
- Serialization/deserialization overhead
- Chatty APIs (N+1 service calls)
Even with gRPC:
> Network ≠ memory
### 4️⃣ Developer Productivity Drop
- Local setup needs [[Docker]], [[Kafka]], DBs, configs
- Onboarding new devs becomes painful
- Hard to refactor cross-cutting logic
### 5️⃣ Premature Microservices
- Services split **by guess**, not by real boundaries
- Boundaries change frequently → painful migrations

|Problem|How Modular Monolith Fixes|
|---|---|
|Ops complexity|Single deployable|
|Data consistency|Single DB, ACID|
|Latency|In-process calls|
|Debugging|Single codebase|
|Refactoring|Compile-time safety|
|Cost|Less infra, fewer services|
# 2️⃣ Design Part
## Core Design Principle
> **Strong logical boundaries, weak physical boundaries**
That means:
- Separate modules
- No direct cross-module access
- Explicit contracts
## 2.1 Codebase / Module Design
### From Microservices → Modules

| Microservice    | Becomes        |
| --------------- | -------------- |
| Order Service   | order-module   |
| Payment Service | payment-module |
| User Service    | user-module    |
### Module rules
- Each module has:
    - API (interfaces)
    - Domain
    - Persistence
- No direct DB access across modules
- Communicate via **interfaces or domain events**
`order-module  ├── api  ├── domain  ├── infra`
**Rule:**
> Only `api` is visible to other modules
## 2.2 UI Design Changes
### Earlier (Microservices)
- Backend for Frontend (BFF)
- [[API Gateway]] → multiple services
- Client does orchestration
### With Modular Monolith
- UI talks to **single backend**
- Backend orchestrates internally
**Benefits**
- Simpler frontend
- Fewer API calls
- No API gateway needed (optional)
## 2.3 Database Design Changes
### Before (Microservices)
- DB per service
- Eventual consistency
- Duplicate data
### After (Modular Monolith)
#### Options
### Option 1: **Single DB, Multiple Schemas (Recommended)**
`order_schema.* payment_schema.* user_schema.*`
- Enforced ownership
- Easier future re-extraction
### Option 2: Single schema, logical separation
- Less ideal but simpler
### Transaction Management
- ACID transactions possible
- No sagas for internal flows
- Still publish events for async needs
## 2.4 Communication Pattern Changes
### Before
- REST / gRPC
- Retries, timeouts
### After
- In-process calls
- Domain events (sync / async)
`orderService.placeOrder() → paymentService.authorize() → inventoryService.reserve()`
No network failures, no serialization.
# 3️⃣ Release & Deployment
## How do you deploy a Modular Monolith?
### Deployment Model
- **Single artifact**
    - One JAR / WAR
    - One container image
### CI/CD
- Single pipeline
- Faster builds
- One rollback unit
---
### Targeted Scaling (Advanced)
- Use **feature toggles**
- Async workloads ([[Kafka]] / queues)
- Thread pool isolation per module
> You scale _load_, not services.
## Rollback Strategies
### Simple Rollback
- Redeploy previous version
- DB backward compatibility is key
### Feature Flags
- Kill feature without redeploy
- Gradual rollout
### Blue-Green / Canary
- Same strategies as microservices
- But fewer moving parts
# 4️⃣ Important Concepts Interviewers Love
## “Is Modular Monolith a Step Back?”
**Strong answer:**
> No. It’s often a **step forward**—microservices are an optimization, not a goal.
## “When would you NOT use Modular Monolith?”
- Very large org (50+ teams)
- Independent release cycles required
- Different tech stacks per domain
- Extreme scalability differences per domain
## “Can we go back to Microservices later?”
**Yes—and that’s the key.**
Modular monolith is:
> **[[Microservices]]-ready architecture without microservices overhead**


[^1]: We migrated from microservices to a modular monolith to reduce operational complexity, improve consistency and performance, and regain development velocity, while still preserving strong domain boundaries that allow future service extraction when scale truly demands it.
