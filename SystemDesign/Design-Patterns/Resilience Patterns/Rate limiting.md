What is rate limiting?
Pros and cons of rate limiting
Usecases of rate limiting
Different ways to do rate limiting
Which case is suitable to what type of rate limiting?

[[Rate limiting]] is a mechanism to control the number of requests a user/system can make in a given time window to protect services from abuse, ensure fair usage, and manage system load.  
It can be implemented via **token bucket, leaky bucket, fixed window, sliding window, or concurrency limits**.  
Choose based on use-case: **APIs (token bucket/sliding window), bursty traffic (token bucket), predictable steady limits (leaky bucket), fairness per user (per-client quota), DDoS protection (global rate limits), resource-heavy ops (concurrency limits).**

### 1. **What is Rate Limiting?**

- It’s the practice of **restricting the number of requests/operations per client (IP, user, API key, etc.) within a defined timeframe**.
- Example: _“Max 100 requests per minute per user.”_

---

### 2. **Pros and Cons**

**✅ Pros**

- Protects system from overload & denial-of-service (DoS).
- Ensures **fair usage** among clients.
- Helps in **cost control** (avoids excessive downstream DB/[[Kafka]] calls).
- Improves system **stability & predictability**.

**❌ Cons**

- Adds **[^1]latency** (throttling/waiting).
- Might block **legitimate bursts** of traffic.
- Complexity in [^2]**distributed setup** (synchronizing counters across multiple nodes).
- Can create **bad UX** if not communicated (e.g., “429 Too Many Requests” needs clear retry-after header).

---

### 3. **Use Cases**

- **Public APIs** (Google Maps, Stripe, Mastercard APIs): prevent abuse.
- **Authentication endpoints**: protect against brute force login attempts.
- **Expensive operations**: file uploads, heavy computations, DB queries.
- **Downstream service protection**: avoid overwhelming external APIs or microservices.
- **Multi-tenant SaaS**:  Enforces **plan-based quotas** (e.g., Free: 100 req/min, Pro: 1,000 req/min, Enterprise: 10,000 req/min).

---

### 4. **Different Ways to Do Rate Limiting**

#### a) **Token Bucket**

- Each client gets tokens (e.g., 100/min). Every request consumes one. Tokens refill at a fixed rate.
- Allows **short bursts**, then smooth refill.
- Libraries: `[[Bucket4j]]`, `[[resilience4j-ratelimiter]]`.

#### b) **Leaky Bucket**

- Requests flow into a bucket but are processed at a **fixed rate** (like a tap dripping).
- Prevents bursts, ensures steady flow.
- Good for **bandwidth shaping** (network APIs).
- Libraries: **[[Bucket4j]]** (can **simulate Leaky Bucket** by configuring refill + scheduling to enforce a **steady drip rate** (like constant throughput).), **Guava RateLimiter**(Uses a **smoothly distributed permit model** (fixed stable rate).)

#### c) **Fixed Window Counter**

- Simple: _“Max 100 requests per minute.”_ Count resets every minute.
- Very easy, but **bursty near window edges** (e.g., 100 in last sec + 100 in next sec).

#### d) **Sliding Window (Log or Rolling Window)**

- Tracks requests in a rolling time window (e.g., last 60s).
- Fairer than fixed window.
- Used by Redis `INCR` + `EXPIRE`.

#### e) **Concurrency Limit**

- Instead of requests/time, it limits **parallel executions** (e.g., max 10 DB connections).
- Useful for **resource-bound APIs**.

#### f) **Distributed Rate Limiting**

- Needed if you have **multiple service instances**.
    
    - Use **Redis** or **Hazelcast** to maintain global counters.
    - Spring Cloud Gateway + Redis RateLimiter (built-in support).

---

### 5. **Which Case is Suitable?**

|**Scenario**|**Best Rate Limiting Strategy**|**Why**|
|---|---|---|
|Public APIs (burst traffic allowed)|Token Bucket|Allows bursts + fairness|
|Payment gateway / banking API|Sliding Window|Prevents abuse, more accurate|
|Network bandwidth / streaming|Leaky Bucket|Smooth, predictable flow|
|Simple per-minute quota (small apps)|Fixed Window|Easy, minimal overhead|
|DB-heavy endpoints|Concurrency Limit|Protects DB from too many connections|
|Multi-node microservices|Distributed (Redis)|Keeps counters consistent|

---

👉 In **[[Spring Boot]]**, common choices are:

- **[[Bucket4j]]** (in-memory + Redis adapter) → fine-grained, production-ready.
- **[[Spring Cloud Gateway]] RateLimiter** ([[Redis]] backend) → for [[API Gateway]] style.
- **[[Resilience4j]] [[Rate limiting]]** → when combined with retries, circuit breakers.




[^1]:  delay between a request and a response, often measured in milliseconds

[^2]: a collection of independent computers, or nodes, that work together over a network to appear as a single, coherent system to the user
