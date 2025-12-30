Gatling Performance Scripting
Tarus.yml file
Load Injection Types.

Gatling is a load testing tool used to measure throughput, latency, and scalability of APIs and applications using asynchronous, event-driven simulations.

|Problem|What Gatling tells|
|---|---|
|Can system handle expected load?|TPS, response times|
|What happens at peak traffic?|Breaking point|
|Is latency stable?|P95 / P99|
|Are there leaks over time?|Soak test|
|Did optimization really help?|Before vs after comparison|
## Key Features of Gatling

| Feature             | Why it matters                 |
| ------------------- | ------------------------------ |
| Asynchronous engine | High concurrency with low CPU  |
| Code-based DSL      | Version controlled, reusable   |
| Detailed reports    | P50, P95, P99, error trends    |
| CI/CD friendly      | Run via Maven, Gradle          |
| Protocol support    | HTTP, WebSocket, gRPC (plugin) |

How Gatling Works (Mental Model)
```
Scenario → Injection Profile → Virtual Users
     ↓             ↓
   Requests     Load Pattern
     ↓
Metrics Collection → HTML Reports

```

## Typical Gatling Use Cases

- API load testing (REST / GraphQL)
- Microservices scalability testing
- Regression performance testing
- Stress & soak testing
- Pre-production validation
# Injection Profiles (Very Important for Interviews)

|Profile|Use case|
|---|---|
|`atOnceUsers`|Smoke test|
|`rampUsers`|Gradual load|
|`constantUsersPerSec`|Steady traffic|
|`rampUsersPerSec`|Stress testing|
|`heavisideUsers`|Spike load|
# What is Taurus? (Context before `taurus.yml`)

**Taurus is a test orchestration tool that provides a YAML-based abstraction over load tools like Gatling, JMeter, Locust, and k6.**

👉 Instead of writing Scala, you define load tests declaratively using `taurus.yml`.

Gatling is the load engine, Taurus is the orchestrator. Taurus allows teams to define load tests declaratively and run them consistently across environments and CI pipelines.”

# When NOT to Use Taurus

- Complex protocol scripting
- Custom session handling logic
- Deep Scala-based simulations
👉 In such cases, write **native Gatling Scala DSL**.