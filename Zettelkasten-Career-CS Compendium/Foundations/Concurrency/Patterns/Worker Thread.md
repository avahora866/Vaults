---
tags:
  - FLASHCARD
  - Concurrency
---
The Worker Thread pattern (most commonly seen as a Thread Pool) is a concurrency pattern that manages a set of reusable threads to execute tasks from a shared queue. This avoids the high overhead of creating and destroying a new thread for every task, leading to improved performance and resource management.

# Explanation
Creating a new thread is computationally expensive. It requires the operating system to allocate memory, set up thread context, and manage its scheduling. If an application needs to perform many small, short-lived tasks, creating a new thread for each one can quickly overwhelm the system and degrade performance.

The Worker Thread pattern solves this by creating a fixed number of worker threads at startup.
- Task Queue: A shared queue (like a BlockingQueue) is created to hold tasks (Runnable or Callable).
- Worker Threads: A pool of threads is created. These threads run in a loop, constantly trying to take a task from the queue.
- Execution: When a new task is submitted, it is added to the queue. An idle worker thread retrieves the task, executes its run() method, and then immediately returns to the queue to wait for the next task.

This decouples the submission of a task from its execution and efficiently reuses threads.

# Code
In modern Java, you should almost never implement this pattern manually. The `java.util.concurrent.ExecutorService` framework provides a powerful and robust implementation.
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

/**
 * A simple task for a worker thread to execute.
 */
class Task implements Runnable {
    private final int taskId;

    public Task(int taskId) {
        this.taskId = taskId;
    }

    @Override
    public void run() {
        // The "work" being done
        System.out.println("Task " + taskId + " is being executed by thread: "
            + Thread.currentThread().getName());
        try {
            Thread.sleep(500); // Simulate work
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        System.out.println("Task " + taskId + " is complete.");
    }
}

public class WorkerThreadPatternDemo {
    public static void main(String[] args) {
        
        // 1. Create a pool of 3 worker threads.
        // This creates the threads and the task queue.
        ExecutorService executor = Executors.newFixedThreadPool(3);

        System.out.println("Submitting 10 tasks to the thread pool...");

        // 2. Submit 10 tasks to the pool's work queue.
        for (int i = 0; i < 10; i++) {
            Task task = new Task(i);
            executor.submit(task);
        }

        // 3. Tell the executor to shut down (it won't accept new tasks)
        executor.shutdown();

        try {
            // Wait for all running tasks to complete
            executor.awaitTermination(1, TimeUnit.MINUTES);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("All tasks are complete.");
    }
}
```

## Code Explanation
- Executors.newFixedThreadPool(3): This is the core of the pattern. It creates an ExecutorService (a thread pool) with 3 worker threads. These threads are created immediately and wait for work.
- executor.submit(task): This places a new Task (which is a Runnable) into the executor's internal work queue. It does not create a new thread.
- Execution Flow: Since there are 10 tasks and only 3 threads, the first 3 tasks are picked up and executed by the threads. As each thread finishes its task, it goes back to the queue to pick up the next available one, until all 10 tasks are complete.
- executor.shutdown(): This is a crucial step. It tells the pool to stop accepting new tasks and to terminate its worker threads after they finish their current tasks. An application will not exit if a running ExecutorService is not shut down.
