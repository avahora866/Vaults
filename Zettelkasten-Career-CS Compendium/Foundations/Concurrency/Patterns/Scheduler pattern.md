---
tags:
  - FLASHCARD
  - Concurrency
---
The Scheduler pattern is a concurrency pattern that schedules a task to be executed at a specific time in the future, or to run periodically at a fixed interval. It involves a "scheduler" object that manages the timing and execution of tasks, often using a background thread.

# Explanation
This pattern is used when you need to control when a task should run, rather than just running it as soon as possible. It is the foundation for timers, cron jobs, and delayed operations.

The pattern typically consists of:
- A Task: The unit of work to be executed (e.g., a Runnable).
- A Scheduler: An object that maintains a queue of scheduled tasks. It has an internal "timer" thread that monitors the queue.
- Scheduling Logic: When a task is submitted, it is added to the queue with its specific execution time (e.g., "run in 5 seconds" or "run every 10 minutes").
- Execution: The scheduler's timer thread checks the queue. When a task's scheduled time arrives, the scheduler moves that task to a worker thread (often from a thread pool) for execution.

This pattern is essential for tasks like:
- Running a nightly backup job at 2:00 AM.
- Sending a "session timeout" warning after 14 minutes of inactivity.
- Polling a database for new data every 30 seconds.

# Code
In Java, this pattern is built into the `java.util.concurrent.ScheduledExecutorService`, which combines the Scheduler pattern with the Thread Pool pattern. You should always use this framework.
```java
import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

/**
 * A simple task to be scheduled.
 */
class ScheduledTask implements Runnable {
    private final int taskId;

    public ScheduledTask(int taskId) {
        this.taskId = taskId;
    }

    @Override
    public void run() {
        System.out.printf("Task %d: Executing at %s on thread %s%n",
            taskId, new Date(), Thread.currentThread().getName());
        
        // If this was a long task, the scheduler would still
        // schedule the next one correctly.
    }
}

public class SchedulerPatternDemo {
    public static void main(String[] args) throws InterruptedException {
        
        // 1. Create a Scheduler service (which is also a thread pool)
        ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);

        System.out.println("Main: Scheduling tasks... Current time: " + new Date());

        // 2. Schedule a task to run ONCE, after a 3-second delay.
        scheduler.schedule(
            new ScheduledTask(1), 
            3, 
            TimeUnit.SECONDS
        );

        // 3. Schedule a task to run REPEATEDLY.
        // It starts after a 5-second delay, and then runs
        // every 2 seconds after that.
        scheduler.scheduleAtFixedRate(
            new ScheduledTask(2), // The task to run
            5,                    // Initial delay
            2,                    // Period between runs
            TimeUnit.SECONDS
        );

        // Let the scheduler run for 20 seconds
        Thread.sleep(20000);

        // 4. Shut down the scheduler
        System.out.println("Main: Shutting down scheduler.");
        scheduler.shutdown();
        scheduler.awaitTermination(1, TimeUnit.MINUTES);
    }
}
```

## Code Explanation
- Executors.newScheduledThreadPool(2): This factory method creates a scheduler. The 2 indicates it maintains a pool of 2 worker threads to execute the tasks once their time is up.
- scheduler.schedule(...): This method schedules a one-shot task. It takes a Runnable, a delay (3), and the unit of time (TimeUnit.SECONDS). The scheduler will wait 3 seconds before moving this task to a worker thread.
scheduler.scheduleAtFixedRate(...): This method schedules a periodic task.
  - new ScheduledTask(2): The task to run.
  - 5, TimeUnit.SECONDS: The initial delay before the first execution.
  - 2, TimeUnit.SECONDS: The period. The scheduler will try to run this task every 2 seconds after the first run.
  - Note: If scheduleAtFixedRate's task takes longer than the period, the next run will wait. For "fixed delay" (e.g., wait 2 seconds after the last one finishes), you would use scheduler.scheduleWithFixedDelay(...).
- scheduler.shutdown(): This is crucial. It tells the scheduler to stop accepting new tasks and to terminate its threads once all scheduled tasks are complete (or in this case, once the main thread exits, as periodic tasks run forever).