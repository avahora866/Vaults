---
tags:
  - Concurrency
  - FLASHCARD
  - "#SECONDARY_BRANCH"
---
# Overheads of concurrency
Concurrency introduces overheads because managing multiple threads or tasks takes extra time and memory beyond just executing code sequentially

|Overhead Type                  |What Happens                                                                                                               |Overhead                                                                                                                                                                                                                         |Impact                                                                                 |Mitigation                                                                                                                           |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
|Thread creation and destruction|Each thread needs its own stack, registers, and metadata (like scheduling info).                                           |Allocating and initializing a stack (often a few MB). OS must register the thread in its scheduler. Creating and destroying threads involves system calls, which are expensive.                                                  |For short-lived tasks, creation/destruction can dominate execution time.               |Use thread pools or task queues instead of constantly creating new threads.                                                          |
|Context switching              |When the CPU switches from one thread to another, it must save and restore the CPU state (registers, stack pointers, etc.).|Dozens to hundreds of CPU cycles per switch. Cache invalidation (loss of locality).                                                                                                                                              |Frequent context switching increases CPU time spent on overhead rather than computation|Reduce the number of runnable threads relative to CPU cores. Use cooperative scheduling where possible (like async programming).     |
|Synchronisation                |Threads accessing shared data must coordinate (e.g., via locks, semaphores, atomic variables).                             |Lock acquisition and release involve atomic CPU instructions. Contention (multiple threads waiting on the same lock) causes blocking and idle time. Cache coherence traffic increases when multiple threads modify nearby memory.|Can cause lock contention, priority inversion, or even deadlocks.                      |Reduce shared state. Use fine-grained locks or lock-free data structures. Design with immutability or message passing where possible.|
|Memory/cache                   |Threads can cause false sharing—when they modify different variables that reside in the same cache line.                   |Frequent invalidation of caches and increased memory latency.                                                                                                                                                                    |Slower data access and reduced parallel speedup.                                       |Align or pad shared variables to avoid false sharing.                                                                                |
|Scheduling and load balancing  |The OS or runtime decides which thread runs next.                                                                          |Time spent in scheduling decisions. Imbalanced workloads can leave cores idle.                                                                                                                                                   |Wasted CPU potential or unpredictable performance.                                     |Use work-stealing or task-based scheduling frameworks (e.g., ForkJoinPool in Java).                                                  |


# Load balancing in concurrent systems
Load balancing in concurrency systems means distributing work evenly across threads or processing units so that all available resources stay busy and no single one becomes a bottleneck.

When multiple threads or tasks run concurrently:
- Some may finish earlier than others
- Some may be assigned heavier workloads
- Some cores might sit idle while others are overloaded.
Load balancing ensures efficient resource usage and shorter total execution time (makespan) by keeping all workers roughly equally busy.

# Techniques for minimizing context switches
Context switching happens when the CPU pauses one thread and starts another. Each switch involves saving and restoring the thread’s registers, stack, and CPU cache state. This process consumes time and interrupts cache locality.

## Limit the Number of Active Threads
One of the most direct ways to reduce context switching is to limit the number of runnable threads. When there are more active threads than available CPU cores, the operating system is forced to switch between them frequently. You can prevent this by matching the thread count to the number of CPU cores.

Avoid creating new threads for every task; instead, use a fixed-size thread pool. This ensures threads are reused efficiently, reducing both creation overhead and unnecessary context switching.

## Use Thread Pools
Thread pools maintain a fixed number of worker threads that handle multiple tasks over their lifetime. Since threads are not constantly created and destroyed, the scheduler has fewer threads to manage. This stability prevents repeated context switches.

## Minimize Blocking and Waiting
Threads often get switched out when they block—such as waiting for I/O or holding a lock. The CPU then context-switches to another thread, increasing overhead. You can reduce this by using non-blocking I/O (like Java NIO) and lock-free or concurrent data structures.

If synchronization is unavoidable, use fine-grained locks or read-write locks to minimize contention. Alternatively, consider asynchronous frameworks such as CompletableFuture or reactive programming models, which rely on fewer threads and non-blocking execution.

## Avoid Excessive Synchronization
Synchronization primitives like synchronized blocks, locks, and semaphores often cause threads to yield and wait. This yields control back to the OS, leading to a context switch. To avoid this, reduce shared mutable state as much as possible.

Design with immutability or message passing in mind, and use atomic variables for lightweight synchronization. For example:
```java
AtomicInteger counter = new AtomicInteger();
counter.incrementAndGet();
```
This avoids locking entirely and allows the thread to continue without interruption.

## Use Cooperative Multitasking
Traditional preemptive multitasking lets the OS interrupt threads at any time, causing frequent context switches. Cooperative multitasking, where threads yield control voluntarily, avoids this. Frameworks that use coroutines, fibers, or virtual threads (such as Java’s Project Loom) provide user-level concurrency with negligible context-switch cost. Since the switch happens in user space rather than the kernel, it is faster and more predictable.

## Keep Threads Affined to Cores
When a thread resumes execution on the same CPU core, it benefits from cached data left over from its previous run. If the OS migrates a thread to a different core, it loses cache locality and forces another context switch. Using CPU affinity—pinning threads to specific cores—reduces this effect.

## Balance Task Granularity
If tasks are too fine-grained, the scheduler must switch between them often, adding unnecessary context switching overhead. Coarser-grained tasks, where each thread performs a larger amount of work before yielding, reduce switching frequency. Adjusting task size or tuning fork-join thresholds helps maintain an efficient balance between concurrency and switching cost.

# Concurrency patterns for optimizing performance (e.g., producer-consumer, fork-join) => Linked notes
Concurrency patterns are used to provide structured, reusable solutions for common problems that arise when multiple tasks run at the same time. 

Some of the common concurrency patterns are listed below:

1. [[Thread local storage]] – Simple to use; each thread gets its own instance of a variable.
2. [[Balking Pattern]] – Concurrency design pattern where an action is only executed if an object is in a particular state.
3. [[Read-Write Model]]– Allowing multiple threads to read data simultaneously while ensuring exclusive access when a thread needs to write data. Slightly more complex, but easy to use via built-in libraries like `ReentrantReadWriteLock`.
4. [[Worker Thread]] – Involves creating threads that repeatedly process tasks but is still easy to implement.
5. [[Thread pool]] – Requires managing multiple threads efficiently; Java’s `ExecutorService` simplifies this.
6. [[Producer-Consumer]] – Requires synchronized queues and coordination between producer and consumer threads.
7. [[Scheduler pattern|Scheduler Pattern]] – Needs careful timing control but is often handled by frameworks like `ScheduledExecutorService`.
8. [[Future Promise|Future/Promise]] – Allows asynchronous execution; handling dependencies and errors adds complexity.
9. [[Fork Join|Fork/Join]] – Requires breaking tasks recursively and merging results, best used with `ForkJoinPool`.
10. [[Reactor|Reactor Pattern]] – Uses an [[Event-Driven Architecture|event-driven]] model, requiring call-back handling and non-blocking I/O.
11. Barrier Pattern – Involves synchronizing multiple threads, requiring careful coordination.
12. [[Actor model]] – Requires message-passing and avoiding shared state, used in Akka and Erlang.
13. [[Proactor Pattern]] – A more complex version of [[Reactor|Reactor]], often requiring OS-level async I/O support.	