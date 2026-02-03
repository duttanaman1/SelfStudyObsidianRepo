What is virtual threads.
Pros cons
Why use Virtual threads instead of reactive programming.
when implementing VT, what should be taken care (DB over load, backpressure mechanisms)

In Java 21, Virtual Threads are a production-ready (GA) feature from Project Loom that allow a thread-per-request model at massive scale by decoupling Java threads from OS threads.

## Why Virtual Threads exist

**Problem with platform threads**

- 1 OS thread ≈ 1 request
- High memory (~1–2 MB stack)
- Blocking I/O kills scalability

**Old workaround**

- CompletableFuture
- Reactive (WebFlux)
- Callback hell

**Virtual Threads fix this**

- Keep **blocking code**
- Scale like async
- Simpler mental model

How Virtual Threads work (key interview concept)
```
Virtual Thread
   ↓ mounted on
Carrier Thread (OS thread)
   ↓ executes
CPU
```

When virtual thread blocks:
- JVM **unmounts** it
- Carrier thread reused
- Virtual thread resumes later
➡ Blocking is **cheap**

```
try (ExecutorService executor =
         Executors.newVirtualThreadPerTaskExecutor()) {

    executor.submit(() -> {
        Thread.sleep(1000);
        return "done";
    });
}

```

## When Virtual Threads shine

✔ I/O-bound workloads  
✔ Microservices  
✔ DB-heavy APIs  
✔ Thread-per-request servers

---

## When NOT to use

❌ CPU-bound workloads  
❌ Tight synchronized blocks  
❌ Legacy native libraries  
❌ Real-time latency-critical systems

