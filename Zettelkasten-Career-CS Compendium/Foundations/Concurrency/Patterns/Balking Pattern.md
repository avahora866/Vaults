---
tags:
  - Concurrency
  - FLASHCARD
---
The Balking pattern is a concurrency design pattern where an object "balks" (refuses) to execute a method or action if it is in an in appropriate state. If an object's state is not ready for the method call, it immediately returns, often with an error or a simple false, rather than waiting for the state to become appropriate.

# Explanation
This pattern is used in systems where:
- An action is only valid during a specific time or in a specific state.
- Waiting for the state to become valid is undesirable or impossible.
- It is better to fail fast and let the caller decide how to retry, if at all.

A common example is an object with a one-time initialization method. If multiple threads call initialize() at the same time, only the first thread should perform the initialization. All subsequent threads that call initialize() while it's in progress, or after it's complete, should "balk"—that is, the method should return immediately without doing anything.

This pattern is simpler than other concurrency patterns because it avoids blocking. It differs from a Guard Block (like wait/notify) because a guard block makes the thread wait until the state is correct, whereas Balking makes the thread give up.

# Code
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

/**
 * This class represents a resource that can only be initialized once.
 */
class OneTimeResource {

    private boolean initialized = false;
    private final Object lock = new Object();

    /**
     * Initializes the resource.
     * If already initializing or initialized, it balks.
     */
    public void init() {
        // Use a lock for a "check-then-act" critical section
        synchronized (lock) {
            // 1. Check the state. If not appropriate, balk.
            if (initialized) {
                System.out.println(Thread.currentThread().getName() + ": Balking! Already initialized.");
                return; // The "balk"
            }

            // 2. Perform the action (the state is valid)
            System.out.println(Thread.currentThread().getName() + ": Initializing resource...");
            try {
                // Simulate a time-consuming initialization
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }

            // 3. Change the state
            this.initialized = true;
            System.out.println(Thread.currentThread().getName() + ": Initialization complete.");
        }
    }

    public boolean isInitialized() {
        synchronized (lock) {
            return initialized;
        }
    }
}

public class BalkingPatternDemo {
    public static void main(String[] args) throws InterruptedException {
        OneTimeResource resource = new OneTimeResource();
        ExecutorService executor = Executors.newFixedThreadPool(5);

        // Submit 5 tasks that all try to initialize the same resource
        for (int i = 0; i < 5; i++) {
            executor.submit(() -> resource.init());
        }

        executor.shutdown();
        executor.awaitTermination(1, TimeUnit.MINUTES);
    }
}
```

## Code Explanation
The init() method first acquires a lock. This is crucial because the "check-then-act" sequence (if (initialized) then this.initialized = true) must be atomic.

The first line inside the synchronized block is the guard condition. It checks if (initialized).

If this condition is true (meaning another thread has already completed or is in the process of initializing), the method immediately hits return. This is the balk. The thread gives up and does not wait.

If the condition is false, the thread proceeds to perform the initialization logic (simulated with Thread.sleep()) and then updates the initialized flag to true.

When you run this code, you will see one thread "Initializing resource..." while the other four "Balking!". This shows the pattern successfully protecting the resource from concurrent initialization without making other threads wait.