JProfiler is a JVM profiling tool used to identify CPU, memory, thread, GC, and lock bottlenecks in Java applications with minimal runtime overhead.

## Why do we use JProfiler?
To **find real performance bottlenecks** in running Java applications that cannot be detected by code review or logs.

## What can JProfiler analyze?
| Area                 | What it shows                                        |
| -------------------- | ---------------------------------------------------- |
| **CPU Profiling**    | Hot methods, call trees, sampling vs instrumentation |
| **Memory Profiling** | Allocation hotspots, retained objects, leaks         |
| **Threads & Locks**  | Deadlocks, blocked threads, lock contention          |
| **GC & Heap**        | GC frequency, pause times, heap usage                |
| **JDBC / I/O**       | Slow queries, connection leaks                       |
| **Exceptions**       | Hot exception paths                                  |
## How JProfiler works (Important)

### Two profiling modes

| Mode                                | Use when               |
| ----------------------------------- | ---------------------- |
| **Sampling (Low overhead)**         | Production / staging   |
| **Instrumentation (High accuracy)** | Local / deep debugging |
## Common Alternatives to JProfiler (with Pros & Cons)

### 1️⃣ VisualVM (Free)
**Best for:** Quick local analysis
### 2️⃣ Java Flight Recorder (JFR)
**Best for:** Production profiling
### 3️⃣Async Profiler (Flamegraphs)
**Best for:** Hardcore performance engineers

## How a Developer Uses JProfiler (Real Workflow)

### Typical workflow

1. Attach JProfiler to JVM
2. Start CPU / Memory recording
3. Reproduce load (API call, batch job)
4. Identify **hotspots**
5. Fix code
6. Re-profile to verify improvement

## CPU Bottleneck Example

### ❌ Problem Code (Classic Mistake)
```
public class OrderService {

    public String processOrders(List<Order> orders) {
        String result = "";
        for (Order order : orders) {
            result += order.getId();  // String concatenation in loop
        }
        return result;
    }
}
```


🔍 What JProfiler Shows
Hotspot: java.lang.StringBuilder.append() (internally)

High CPU time in processOrders

Call tree shows repeated string allocations

```public String processOrders(List<Order> orders) {
    StringBuilder sb = new StringBuilder();
    for (Order order : orders) {
        sb.append(order.getId());
    }
    return sb.toString();
}
```

## Memory Leak Example

### ❌ Problem Code
```public class Cache {
    private static final Map<String, byte[]> CACHE = new HashMap<>();

    public static void put(String key, byte[] data) {
        CACHE.put(key, data);
    }
}

```
### 🔍 What JProfiler Shows

- Heap keeps growing
    
- **Retained heap dominated by `HashMap`**
    
- Reference chain shows static `CACHE`
Fix
```private static final Map<String, byte[]> CACHE =
        new LinkedHashMap<>(1000, 0.75f, true) {
            protected boolean removeEldestEntry(Map.Entry eldest) {
                return size() > 1000;
            }
        };

```

### ❌ Problem Code (Simplified)
```CompletableFuture
    .supplyAsync(() -> fetchData())
    .thenApply(data -> transform(data))
    .join(); // blocking inside request thread

```
Under Gatling load:

- Request threads block
- Thread pool exhausted
- Latency increased
## 6️⃣ Fix Implemented

### ✅ Improved Async Design
```
CompletableFuture<String> future =
    CompletableFuture.supplyAsync(() -> fetchData(), executor)
        .thenApplyAsync(this::transform, executor);

return future; // non-blocking

```
