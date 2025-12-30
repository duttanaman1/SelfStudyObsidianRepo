A BlockingQueue is a thread-safe queue that blocks producers when the queue is full and consumers when the queue is empty, enabling safe producer–consumer coordination.

## Common BlockingQueue Implementations

| Queue                   | 특징                      | Use case                     |
| ----------------------- | ----------------------- | ---------------------------- |
| `ArrayBlockingQueue`    | Bounded, array-based    | Backpressure, fixed capacity |
| `LinkedBlockingQueue`   | Optional bound          | Default choice               |
| `PriorityBlockingQueue` | Priority ordering       | Task scheduling              |
| `DelayQueue`            | Time-based availability | Scheduled tasks              |
| `SynchronousQueue`      | Zero capacity           | Direct handoff               |
| `LinkedTransferQueue`   | High throughput         | Advanced producer–consumer   |

```
BlockingQueue<String> queue = new ArrayBlockingQueue<>(5);

// Producer
new Thread(() -> {
    try {
        queue.put("task1"); // blocks if full
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
}).start();

// Consumer
new Thread(() -> {
    try {
        String task = queue.take(); // blocks if empty
        System.out.println(task);
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
}).start();

```
