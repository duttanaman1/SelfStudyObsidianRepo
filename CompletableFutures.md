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


> **Adopt `CompletableFuture` for non-blocking, I/O-bound, composable async workflows; avoid it for CPU-bound, simple synchronous flows, or when structured concurrency is required.**