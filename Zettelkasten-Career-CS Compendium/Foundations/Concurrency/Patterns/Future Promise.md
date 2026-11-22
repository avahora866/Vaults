---
tags:
  - FLASHCARD
  - Concurrency
---
The Future/Promise pattern is a concurrency model for handling asynchronous results.

- A Future is a read-only placeholder (a proxy) for a value that is not yet known, typically because its computation is not complete.
- A Promise is the writable, settable object that "fulfills" the Future by providing the result (or an error) once the computation is finished.

# Explanation
This pattern decouples a task's invocation from its result. When you call an asynchronous method, it doesn't block and wait for the long-running task (like a network call or database query) to finish. Instead, it immediately returns a Future object.

Your main thread can continue doing other work.
- Without Blocking: You can attach a callback (e.g., .thenAccept()). This callback function will be automatically executed by a thread from the pool only when the result is finally available.
- With Blocking: You can call .get() on the Future. This will block your current thread, forcing it to wait until the result is available if it isn't already.

This pattern is fundamental for building responsive, non-blocking applications. The "promise" is the object given to the asynchronous worker, which it uses to complete the future, thereby fulfilling its promise to deliver a result.

# Code
Java's `CompletableFuture` (since Java 8) is the standard implementation of this pattern.
```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class FuturePromisePatternDemo {

    // 1. Create a thread pool for our async tasks
    private static final ExecutorService executor = Executors.newFixedThreadPool(2);

    /**
     * This method simulates a long-running task (like a network call).
     * It immediately returns a "promise" of a String.
     */
    public static CompletableFuture<String> fetchRemoteData() {
        
        // 2. supplyAsync runs the task in the thread pool and returns a
        // CompletableFuture immediately.
        return CompletableFuture.supplyAsync(() -> {
            // This code runs on a worker thread
            try {
                System.out.println(Thread.currentThread().getName() 
                    + ": Starting to fetch data...");
                Thread.sleep(2000); // Simulate 2-second network delay
                System.out.println(Thread.currentThread().getName() 
                    + ": ...Finished fetching data.");
                
                // 3. This return value "fulfills" the promise,
                // completing the future.
                return "Async Result Data";
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return "Error";
            }
        }, executor);
    }

    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName() 
            + ": Making async request...");
        
        // 4. Call the method, which returns the Future immediately.
        CompletableFuture<String> future = fetchRemoteData();

        System.out.println(Thread.currentThread().getName() 
            + ": Main thread is not blocked, doing other work.");

        // 5. Attach a non-blocking callback.
        // This code will run later, when the future is completed.
        future.thenAccept(result -> {
            System.out.println(Thread.currentThread().getName() 
                + ": CALLBACK EXECUTED. Result is: " + result);
        });

        // We use join() here just to stop the main thread from exiting
        // before the async task and its callback can finish.
        // .join() is a blocking call, similar to .get() but unchecked.
        future.join();
        
        executor.shutdown();
    }
}
```

## Code Explanation
- ExecutorService: We create a thread pool to run our asynchronous tasks.
- CompletableFuture.supplyAsync(..., executor): This is the core of the pattern. We give it a Supplier (a function that returns a value) and our executor. It immediately hands back a CompletableFuture<String> and schedules the Supplier to run on a thread from the pool.
- Task Logic: The code inside the lambda (including the Thread.sleep) runs on a separate worker thread. The return "Async Result Data"; call is what completes the future, making the result available.
- future variable: In main, this variable holds the placeholder. The main thread is not blocked and continues to the next line.
- .thenAccept(result -> ...): This is the non-blocking callback. We attach a Consumer to the future. We are telling the future, "When you eventually have a result, please give it to this lambda function and have one of the threads execute it." This is why the "CALLBACK EXECUTED" message appears after the main thread has finished its work.