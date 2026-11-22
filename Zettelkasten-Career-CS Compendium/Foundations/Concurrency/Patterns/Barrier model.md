---
tags:
  - Concurrency
  - FLASHCARD
---
The **Barrier** pattern is a synchronization method that forces a group of threads to all wait at a specific point in their execution. No thread is allowed to proceed *past* the barrier until *all* threads in the group have arrived at that barrier. Once the last thread arrives, the barrier "trips" and all waiting threads are released simultaneously.

# Explanation
This pattern is used to coordinate different phases of a computation. It acts as a rendezvous or meeting point for threads.

Imagine a team of runners in a multi-lap relay race. The entire team must finish the first lap before *any* team member can start the second lap. The finish line for lap 1 is the barrier.

1. **Arrival:** As each runner (thread) finishes lap 1 (a phase of work), they arrive at the barrier and must wait.
2. **Waiting:** Runners who finish early are blocked. They cannot start lap 2.
3. **Release:** Only when the *last* runner (thread) arrives at the barrier does the barrier break. All runners are then released at the same time to start lap 2 (the next phase of work).

This is crucial in parallel algorithms where one phase depends on the results of all threads from the previous phase (e.g., in scientific computing or parallel simulations).

# Code
In Java, this pattern is implemented by the `java.util.concurrent.CyclicBarrier`.

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

/**
 * A worker task that performs work in two phases.
 */
class Worker implements Runnable {
    private final int workerId;
    private final CyclicBarrier barrier;

    public Worker(int workerId, CyclicBarrier barrier) {
        this.workerId = workerId;
        this.barrier = barrier;
    }

    @Override
    public void run() {
        try {
            // --- Phase 1 ---
            System.out.println("Worker " + workerId + " starting Phase 1.");
            Thread.sleep(1000 + (long) (Math.random() * 2000)); // Simulate work
            System.out.println("Worker " + workerId + " FINISHED Phase 1, waiting at barrier...");

            // 1. Arrive at the barrier and wait for others.
            barrier.await();

            // --- Phase 2 ---
            // This code will only run after ALL threads have hit barrier.await()
            System.out.println("Worker " + workerId + " starting Phase 2.");
            Thread.sleep(1000 + (long) (Math.random() * 1000));
            System.out.println("Worker " + workerId + " FINISHED Phase 2.");

        } catch (InterruptedException | BrokenBarrierException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class BarrierPatternDemo {
    public static void main(String[] args) {
        final int NUM_WORKERS = 3;

        // 2. Create a barrier that will wait for 3 "parties" (threads).
        CyclicBarrier barrier = new CyclicBarrier(NUM_WORKERS, () -> {
            // This optional Runnable runs once, after the last thread arrives
            // but BEFORE the other threads are released.
            System.out.println("\n*** BARRIER TRIPPED! All workers finished Phase 1. Releasing threads. ***\n");
        });

        ExecutorService executor = Executors.newFixedThreadPool(NUM_WORKERS);
        
        // 3. Start all 3 workers.
        for (int i = 0; i < NUM_WORKERS; i++) {
            executor.submit(new Worker(i, barrier));
        }

        try {
            executor.shutdown();
            executor.awaitTermination(1, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

## Code Explanation
1. **`barrier.await()`**: This is the critical call. When a thread calls this, its `workerId` is counted, and the thread is blocked.
2. **`new CyclicBarrier(3, ...)`**: We initialize the `CyclicBarrier` to wait for 3 "parties" (threads). The second argument is an optional `Runnable` that executes *when the barrier is tripped* (when the 3rd thread arrives).
3. **Execution Flow**: Even though some workers will finish Phase 1 quickly, they will all block at `barrier.await()`. You will see all three "FINISHED Phase 1" messages print *before* any "starting Phase 2" messages.
4. **"Cyclic"**: The name `CyclicBarrier` is used because after the barrier is tripped, it automatically resets. This means it can be reused in a loop for subsequent phases (e.g., at the end of Phase 2, Phase 3, etc.).