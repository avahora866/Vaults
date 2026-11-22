---
tags:
  - Concurrency
---
The Producer-Consumer pattern is a classic concurrency solution that decouples a "Producer" (the code that creates work) from a "Consumer" (the code that does the work). The two are connected by a shared, thread-safe queue. The producer adds tasks to the queue, and the consumer independently removes and executes them.

# Explanation
This pattern smooths out workflows where task creation and task completion happen at different rates.

- The Producer: Its only job is to generate tasks and add them to the queue. If the queue is full, it blocks (waits) until the consumer makes space.
- The Queue: This is the key component. It must be thread-safe (like a BlockingQueue) to handle concurrent access. It acts as a buffer, holding tasks until the consumer is ready for them.
- The Consumer: Its only job is to take tasks from the queue and process them. If the queue is empty, it blocks (waits) until the producer adds a new task.

This separation of concerns simplifies logic and improves throughput:
- Decoupling: The producer doesn't need to know anything about the consumer (and vice versa), only about the shared queue.
- Load Balancing: The queue acts as a buffer. If the producer is "bursty" (creates many tasks at once), it can fill the queue, and the consumer can process them at its own steady pace.
- Backpressure: The fixed size of the queue naturally handles "backpressure." If the consumer is slow and the queue fills up, the producer is forced to slow down, preventing the system from running out of memory.

# Code
Java's `java.util.concurrent.BlockingQueue` is the perfect tool for this pattern, as it handles all the waiting and notifying automatically.
```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

/**
 * Producer class - produces tasks (integers) and puts them in the queue.
 */
class Producer implements Runnable {
    private final BlockingQueue<Integer> queue;
    private int counter = 1;

    public Producer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            while (true) {
                // 1. Produce a new task
                int task = counter++;
                System.out.println("Producer: Creating task " + task);
                
                // 2. Put the task on the queue.
                // If the queue is full, this call BLOCKS until space is free.
                queue.put(task); 
                
                Thread.sleep(100); // Simulate time to create next task
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

/**
 * Consumer class - takes tasks from the queue and consumes them.
 */
class Consumer implements Runnable {
    private final BlockingQueue<Integer> queue;

    public Consumer(BlockingQueue<Integer> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        try {
            while (true) {
                // 3. Take a task from the queue.
                // If the queue is empty, this call BLOCKS until a task is available.
                int task = queue.take();

                // 4. Consume the task
                System.out.println("Consumer: Processing task " + task);
                Thread.sleep(500); // Simulate time to process task
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}

public class ProducerConsumerDemo {
    public static void main(String[] args) {
        // 5. Create the shared queue with a fixed capacity of 5.
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<>(5);

        // Start one producer thread
        new Thread(new Producer(queue), "Producer-1").start();

        // Start two consumer threads
        new Thread(new Consumer(queue), "Consumer-1").start();
        new Thread(new Consumer(queue), "Consumer-2").start();
    }
}
```

## Code Explanation
- Producer (Producer.run): The producer generates a task (an int).
- queue.put(task): This is the producer's key call. It adds the task to the queue. If the queue is full (it has 5 items), this line will block the producer thread, automatically waiting until a consumer takes an item.
- queue.take(): This is the consumer's key call. It removes a task from the queue. If the queue is empty, this line will block the consumer thread, automatically waiting until a producer adds an item.
- Consumer (Consumer.run): After taking the task, the consumer processes it (simulated by Thread.sleep). In this example, the producer is fast (100ms) and the consumers are slow (500ms).
- ArrayBlockingQueue<>(5): This is the shared buffer. Its capacity of 5 is what creates the backpressure. You will see the producer create 5 tasks quickly, fill the queue, and then be forced to wait, only producing a new task each time a consumer finishes one.
