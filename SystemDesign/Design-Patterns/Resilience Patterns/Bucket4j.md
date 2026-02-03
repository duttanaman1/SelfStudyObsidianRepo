Bucket4j is a Java [[Rate limiting]] library based on the Token Bucket algorithm that enforces request limits with high precision and supports in-memory and distributed backends ([[Redis]], [[Hazelcast]], etc.).
## Why Bucket4j?

Use Bucket4j when you need:
- API [[Rate limiting]] (per user / IP / API key)
- Protection against abuse & traffic spikes
- Precise control (burst + refill rules)
- Distributed rate limiting across multiple instances
- Non-blocking, thread-safe performance
## Core concept: Token Bucket Algorithm

- A **bucket** has a fixed capacity
- Tokens are **refilled at a fixed rate**
- Each request **consumes tokens**
- If no tokens → request is **rejected (429)**
👉 Allows **bursts**, unlike leaky bucket or fixed window counters

Key Components in Bucket4j
1. **Bandwidth**
   *Bandwidth limit = Bandwidth.classic(*
    *100,*                 
    *Refill.greedy(100, Duration.ofMinutes(1))*
*);*

2. **Bucket**
   *Bucket bucket = Bucket4j.builder()*
        *.addLimit(limit)*
        *.build();*

3. **Token Consumption**
   *if (bucket.tryConsume(1)) {*
    *// allow request*
*} else {*
    *// reject (HTTP 429)*
*}*


## Distributed [[Rate limiting|Rate Limiting]] (IMPORTANT for interviews)

### Problem

In-memory buckets ❌ don’t work with:

- Multiple pods
- Load balancers
- Auto-scaling
### Solution

Use **Bucket4j + [[Redis]] / [[Hazelcast]] / Ignite**
```
ProxyManager<String> proxyManager =
    LettuceBasedProxyManager.builderFor(redisClient)
        .build();

Bucket bucket = Bucket4j.extension(Redis.class)
    .builder()
    .addLimit(limit)
    .build(proxyManager, "user:123");

```