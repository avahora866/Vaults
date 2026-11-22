---
tags:
  - FLASHCARD
  - Concurrency
---
The Thread Pool pattern is a concurrency pattern that manages a collection (a "pool") of reusable worker threads. This pattern avoids the high overhead of creating and destroying a new thread for every task. Instead, tasks are submitted to a shared queue, and the threads in the pool continuously pull tasks from the queue and execute them.

This pattern is identical to the Worker Thread pattern. The two terms are used interchangeably, with "Thread Pool" being the more common name for the implementation.

# Explanation
Creating a new thread is computationally expensive. It requires interaction with the operating system, allocation of a private stack (memory), and context switching. If an application needs to perform many small, short-lived tasks (like handling web requests or processing data), creating a new thread for each task would be highly inefficient and could lead to system instability.

The Thread Pool pattern solves this by creating a fixed number of threads at startup.
- Task Queue: A central queue (typically a BlockingQueue) holds all the tasks (Runnable or Callable) that need to be run.
- Worker Threads: A pool of threads is created. These threads run in a loop, waiting to take the next available task from the queue.
- Execution: When a new task is submitted, it is simply added to the queue. An idle worker thread will eventually pick it up, execute its run() method, and then loop back to the queue to wait for another task.

This decouples task submission from task execution and allows for fine-grained control over how many threads are active, preventing resource exhaustion.

# Code
In Java, this pattern is built directly into the language via the `java.util.concurrent.ExecutorService` framework. You should use this framework instead of building your own thread pool.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

/**
 * A simple task for a worker thread to execute.
 */
class PooledTask implements Runnable {
    private final int taskId;

    public PooledTask(int taskId) {
        this.taskId = taskId;
    }

    @Override
    public void run() {
        // The "work" being done
        System.out.println("Task " + taskId + " starting -> Thread: "
            + Thread.currentThread().getName());
        try {
            Thread.sleep(500); // Simulate work duration
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println("Task " + taskId + " complete -> Thread: "
            + Thread.currentThread().getName());
    }
}

public class ThreadPoolPatternDemo {
    public static void main(String[] args) {
        
        // 1. Create a Thread Pool with 4 worker threads.
        // This is the "pool".
        ExecutorService threadPool = Executors.newFixedThreadPool(4);

        System.out.println("Submitting 12 tasks to the thread pool...");

        // 2. Submit 12 tasks to the pool's work queue.
        for (int i = 1; i <= 12; i++) {
            PooledTask task = new PooledTask(i);
            threadPool.submit(task);
        }

        // 3. Shut down the pool.
        // This stops it from accepting new tasks and tells
        // the worker threads to exit once the queue is empty.
        threadPool.shutdown();

        try {
            // Wait for all running tasks to complete
            threadPool.awaitTermination(1, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("All tasks finished. Thread pool shut down.");
    }
}
```

## Code Explanation
- Executors.newFixedThreadPool(4): This is the factory method that creates the thread pool. It instantiates an ExecutorService with 4 worker threads and an internal task queue.
- threadPool.submit(task): This method adds a Runnable task to the work queue. It does not create a new thread. The method returns immediately, and an available worker thread from the pool will pick up the task when it can.
- Execution Flow: With 12 tasks and 4 threads, the pool will execute 4 tasks in parallel. As each task finishes, the thread that ran it will immediately pick up the next available task from the queue (e.g., Task 5, Task 6, etc.). This continues until the queue is empty.
- threadPool.shutdown(): This is a critical step. It signals to the pool that no new tasks will be submitted. The worker threads will finish their current tasks and any tasks still in the queue, and then they will terminate. An application will not exit if a thread pool is not shut down.