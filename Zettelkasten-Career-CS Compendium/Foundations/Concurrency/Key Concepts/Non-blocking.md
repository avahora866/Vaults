---
tags:
  - Concurrency
  - FLASHCARD
---
Non-blocking refers to a concurrent programming design where threads can make progress **without waiting on locks** held by other threads. In **traditional lock-based designs**, if a thread tries to acquire a lock that another thread holds, it must **block (pause) until the lock is released**. **Non-blocking algorithms** avoid this by using [[Atomicity|atomic operations]] (like [[Compare and Swap (CAS) operations|compare-and-swap]]) instead of locks to update shared state.

The main advantage is **reduced contention** and **improved responsiveness**, especially in highly concurrent systems. Since threads don’t block each other, the system can continue making progress even if some threads are delayed or paused.

Non-blocking algorithms are often categorized into three levels:

- **Obstruction-free** – a thread can complete its operation if it runs alone, but no guarantees under contention.
- **Lock-free** – the system as a whole continues making progress even if some threads stall.
- **Wait-free** – every thread is guaranteed to complete its operation in a bounded number of steps.

In practice, non-blocking designs are common in high-performance libraries, concurrent queues, and real-time systems where avoiding thread suspension is critical.

# Obstruction-free

Relies on atomic compare-and-swap (CAS), but makes no guarantees under contention. If two threads constantly collide, neither may make progress.

```java
class ObstructionFreeCounter {
    private final AtomicInteger value = new AtomicInteger(0);

    public void increment() {
        int oldVal = value.get();
        int newVal = oldVal + 1;
        value.compareAndSet(oldVal, newVal); 
        // succeeds only if no one else changed value
        // if CAS fails, thread just gives up (no retry)
    }

    public int get() {
        return value.get();
    }
}
```

- If one thread runs alone, increment always succeeds.
- Under contention, multiple failed CAS attempts may cause operations to “spin” indefinitely.

# Lock-free
Guarantees that **some thread** always makes progress. Uses CAS with retries: even if one thread fails, eventually another thread succeeds.
```java
class LockFreeCounter {
    private final AtomicInteger value = new AtomicInteger(0);

    public void increment() {
        while (true) {
            int oldVal = value.get();
            int newVal = oldVal + 1;
            if (value.compareAndSet(oldVal, newVal)) {
                return; // success
            }
            // retry until success
        }
    }

    public int get() {
        return value.get();
    }
}
```
- At least one thread will complete its operation, ensuring system-wide progress.
- A single unlucky thread could starve if it keeps losing the race.

# Wait-free
Guarantees **every thread** completes in a bounded number of steps. One common approach is to give each thread its own slot and then combine results.
```java
class WaitFreeCounter {
    private final AtomicIntegerArray localCounters;
    private final int numThreads;

    public WaitFreeCounter(int numThreads) {
        this.numThreads = numThreads;
        this.localCounters = new AtomicIntegerArray(numThreads);
    }

    public void increment(int threadId) {
        // Each thread updates only its own slot
        localCounters.incrementAndGet(threadId);
    }

    public int get() {
        int sum = 0;
        for (int i = 0; i < numThreads; i++) {
            sum += localCounters.get(i);
        }
        return sum;
    }
}
```
- Each thread writes only to its own slot → guaranteed to succeed without retries.
- `get()` aggregates all slots.
- This ensures fairness and bounded completion time
---
- **Obstruction-free** may stall if there’s contention.
- **Lock-free** ensures the counter keeps increasing overall, but one thread might starve.
- **Wait-free** ensures every thread gets its increment counted quickly and reliably.