`Future` represents the result of an async computation, while `CompletableFuture`(Java 8+) allows non-blocking, composable, and callback-driven async programming.
```
ExecutorService executor = Executors.newFixedThreadPool(1);

Future<Integer> future = executor.submit(() -> {
    Thread.sleep(1000);
    return 10;
});

// BLOCKS
Integer result = future.get();
System.out.println(result);

```

```
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> {
        sleep(1000);
        return 10;
    });

cf.thenAccept(result -> System.out.println(result));

```

Chaining operations (`thenApply`)
```
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10)
        .thenApply(x -> x * 2)
        .thenApply(x -> x + 5);

System.out.println(cf.join()); // 25

```

Async Chaining (`thenCompose`) ⭐ (Very important)
```
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10)
        .thenCompose(x ->
            CompletableFuture.supplyAsync(() -> x * 2)
        );

System.out.println(cf.join()); // 20

```

Combine independent futures (`thenCombine`)
```
CompletableFuture<Integer> f1 =
    CompletableFuture.supplyAsync(() -> 10);

CompletableFuture<Integer> f2 =
    CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> combined =
    f1.thenCombine(f2, Integer::sum);

System.out.println(combined.join()); // 30

```

Running multiple tasks (`allOf`, `anyOf`)
```
CompletableFuture<Void> all =
    CompletableFuture.allOf(f1, f2);

all.join();

CompletableFuture<Object> any =
    CompletableFuture.anyOf(f1, f2);

System.out.println(any.join());

```

Exception handling (must know)
exceptionally
```
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> {
        throw new RuntimeException("fail");
    }).exceptionally(ex -> 0);

System.out.println(cf.join()); // 0

CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10)
        .handle((res, ex) -> ex == null ? res : 0);

```

Custom Executor (production best practice)
```
ExecutorService executor = Executors.newFixedThreadPool(10);

CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10, executor);

```

___

# 1️⃣ `handle()` vs `handleAsync()` vs `whenComplete()`

## 🔹 `handle()`
- Transforms result
- Runs on **same thread that completed previous stage**
- Returns a new value

```
CompletableFuture<String> future =
    CompletableFuture.supplyAsync(() -> "data")
        .handle((res, ex) -> {
            if (ex != null) return "fallback";
            return res + " processed";
        });
```

✔ Always runs
✔ Can recover from error
✔ Produces new result

## 🔹 `handleAsync()`
- Same as `handle()`
- But runs in **different thread ([[ForkJoinPool]].commonPool by default)**

`.handleAsync((res, ex) -> res + " processed")`

✔ Offloads heavy transformation
✔ Prevents blocking previous stage thread

You can also provide executor:

`.handleAsync((res, ex) -> res, executor);`

## 🔹 `whenComplete()`

```
.whenComplete((res, ex) -> {
    if (ex != null)
        log.error("Failed", ex);
});
```

✔ Used for logging / metrics
✔ Result passes through unchanged
❌ Cannot transform result

## Quick Comparison

|Method|Changes Result?|Recovers Error?|Thread|
|---|---|---|---|
|handle|✅ Yes|✅ Yes|Same thread|
|handleAsync|✅ Yes|✅ Yes|Other thread|
|whenComplete|❌ No|❌ No|Same thread|
|whenCompleteAsync|❌ No|❌ No|Other thread|

___

# 2️⃣ Sync vs Async Variants

Example:

`.thenApply() .thenApplyAsync()`

## 🔹 Sync (`thenApply`)
Runs in **same thread that completed previous stage**
## 🔹 Async (`thenApplyAsync`)
Runs in:
- `ForkJoinPool.commonPool()` by default
- OR custom executor if provided

Better when:
- Transformation is heavy
- Avoid blocking I/O thread
- Avoid blocking common pool worker

---

### Interview Rule of Thumb

Use:

- Sync for lightweight CPU operations
- Async for blocking/heavy logic

---

# 3️⃣ Thread Execution Model (Very Important)
This is where senior engineers shine.
## Case 1: `supplyAsync()`

`CompletableFuture.supplyAsync(...)`

Runs in:

- `ForkJoinPool.commonPool()`
    UNLESS custom executor provided.

---

## Case 2: Non-Async Continuation

`supplyAsync(...).thenApply(...)`

Execution depends on **who completes previous stage**.

If previous stage completes in:

- ForkJoin worker thread → `thenApply` runs in same worker thread
- Manually completed future → runs in caller thread

So:
👉 Non-async stages execute in thread that completes previous stage.

---

## Case 3: Async Continuation

`.thenApplyAsync(...)`

Always runs in:

- Common pool
    OR
    
- Provided executor

Independent of previous thread.

---

# ⚠️ Very Important Production Insight

If you do this:

`CompletableFuture.supplyAsync(...)     .thenApply(res -> {         Thread.sleep(5000); // blocking         return res;     });`

You are:

❌ Blocking ForkJoin worker
❌ Reducing parallelism
❌ Risking thread starvation

Better:

`.thenApplyAsync(res -> heavyWork(), customExecutor)`

---

# 4️⃣ Visual Thread Flow

## Non-Async Chain

`ForkJoin Thread-1    ↓ supplyAsync    ↓ thenApply    ↓ thenCombine`

Same thread continues execution.

---

## Async Chain

`ForkJoin Thread-1    ↓ supplyAsync  ForkJoin Thread-3    ↓ thenApplyAsync`

Switches threads.

---

# 5️⃣ Senior-Level Interview Insight
CompletableFuture is:

- NOT automatically parallel at every stage
- Continuations are lazy-triggered
- Thread choice depends on async vs sync variant

The biggest mistake juniors make:

> Thinking every stage runs in new thread.

It does NOT.

---

# 🎯 Interview-Ready Summary

> “Non-async stages run in the thread that completes the previous stage, while async variants offload to the common pool or a custom executor. `handle()` transforms results and can recover errors, whereas `whenComplete()` is for side-effects only. Understanding this prevents thread starvation and latency amplification.”

> **Adopt `CompletableFuture` for non-blocking, I/O-bound, composable async workflows; avoid it for CPU-bound, simple synchronous flows, or when structured concurrency is required.**
