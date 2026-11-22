---
tags:
  - FLASHCARD
  - Concurrency
---
The Fork-Join pattern is a "divide and conquer" algorithm for parallel processing. It breaks a large task ("forks") into smaller, independent sub-tasks, which are then run recursively. Once the sub-tasks are complete, it "joins" their results to form the final result.

This pattern is specifically designed for CPU-bound tasks and is built around a special thread pool called a ForkJoinPool, which uses a work-stealing algorithm for high efficiency.

# Explanation
This pattern is ideal for problems that can be solved recursively, like parallel sorting, searching, or complex calculations on large datasets.

Here is the process:
- Fork (Divide): A main task is given to the ForkJoinPool. The task checks if its workload is small enough to compute directly (the "base case"). If not, it forks itself into two or more independent sub-tasks.
- Recursive Forking: These sub-tasks then do the same check. They are forked recursively until all the pieces of work are smaller than the base-case threshold.
- Work-Stealing: The ForkJoinPool assigns tasks to its worker threads. Each thread has its own task queue. If a thread finishes its work and its queue is empty, it will steal a task from the end of another thread's queue. This ensures all CPU cores stay busy and balances the load.
- Join (Conquer): As each sub-task finishes, it returns its result. The parent task, which was waiting, "joins" the results from its children, combines them, and passes its own result up the chain until the main task is complete.

# Code
In Java, you implement this by creating a `ForkJoinPool` and a custom task class that extends `RecursiveTask<V>` (if it returns a value) or `RecursiveAction` (if it does not).

This example sums all the numbers in a large array in parallel.
```java
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;

/**
 * A task that sums a portion of an array.
 * It extends RecursiveTask because it returns a value (the sum).
 */
class ArraySumTask extends RecursiveTask<Long> {

    // 1. A threshold to decide when to stop splitting.
    private static final int THRESHOLD = 1000;
    private final int[] array;
    private final int start;
    private final int end;

    public ArraySumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        int length = end - start;

        // 2. Base Case: If the task is small enough, compute it directly.
        if (length <= THRESHOLD) {
            long sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        }

        // 3. Fork: If the task is too large, split it in two.
        int mid = start + (length / 2);
        ArraySumTask leftTask = new ArraySumTask(array, start, mid);
        ArraySumTask rightTask = new ArraySumTask(array, mid, end);

        // 4. Asynchronously fork the left task (run it on another core if possible)
        leftTask.fork();

        // Compute the right task on the current thread
        long rightResult = rightTask.compute();
        
        // 5. Join: Wait for the forked left task to finish and get its result
        long leftResult = leftTask.join();

        // Return the combined result
        return leftResult + rightResult;
    }
}

public class ForkJoinPatternDemo {
    public static void main(String[] args) {
        // Create a large array
        int n = 1_000_000;
        int[] array = new int[n];
        for (int i = 0; i < n; i++) {
            array[i] = 1;
        }

        // 6. Get the common ForkJoinPool (shared across the application)
        ForkJoinPool pool = ForkJoinPool.commonPool();

        // 7. Create the main task for the entire array
        ArraySumTask mainTask = new ArraySumTask(array, 0, array.length);

        // 8. Start the main task and get the final result
        long sum = pool.invoke(mainTask);

        System.out.println("Pool parallelism: " + pool.getParallelism());
        System.out.println("Total sum: " + sum);
        
        pool.shutdown();
    }
}
```

## Code Explanation
- THRESHOLD: This is the crucial "base case" size. Tasks smaller than this are not split further. Tuning this value is important for performance.
- compute() Base Case: The if block handles the "conquer" part for a small piece of work. It runs a simple for loop.
- compute() Fork: The else block handles the "divide" part. It creates two new ArraySumTasks, splitting the array portion in half.
- leftTask.fork(): This is the "fork." It schedules the leftTask to be run asynchronously, pushing it onto the current thread's work queue, where another thread might steal it.
- rightTask.compute() / leftTask.join(): This is the "join." The current thread directly computes the rightTask (a recursive call). Meanwhile, it waits for the forked leftTask to be completed by another thread. join() blocks until the result is ready and then returns it.
- ForkJoinPool.commonPool(): This is a static, shared thread pool available to the entire JVM. It is the recommended way to get a ForkJoinPool.
- mainTask: We create the first, largest task that represents the entire problem.
- pool.invoke(mainTask): This call submits the main task to the pool and waits for it to be fully completed, returning the final aggregated result.