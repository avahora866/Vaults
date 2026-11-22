---
tags:
  - Concurrency
  - CHECKLIST
---
Beginner Level

1. [[Thread local storage]] – Simple to use; each thread gets its own instance of a variable.
2. [[Balking Pattern]] – Concurrency design pattern where an action is only executed if an object is in a particular state.
3. [[Read-Write Model]]– Allowing multiple threads to read data simultaneously while ensuring exclusive access when a thread needs to write data. Slightly more complex, but easy to use via built-in libraries like `ReentrantReadWriteLock`.
4. [[Worker Thread]] – Involves creating threads that repeatedly process tasks but is still easy to implement.

Intermediate Level

1. [[Thread pool]] – Requires managing multiple threads efficiently; Java’s `ExecutorService` simplifies this.
2. [[Producer-Consumer]] – Requires synchronized queues and coordination between producer and consumer threads.
3. [[Scheduler pattern|Scheduler Pattern]] – Needs careful timing control but is often handled by frameworks like `ScheduledExecutorService`.
4. [[Future Promise|Future/Promise]] – Allows asynchronous execution; handling dependencies and errors adds complexity.

Advanced Level

1. [[Fork Join|Fork/Join]] – Requires breaking tasks recursively and merging results, best used with `ForkJoinPool`.
2. [[Reactor|Reactor Pattern]] – Uses an [[Event-Driven Architecture|event-driven]] model, requiring call-back handling and non-blocking I/O.
3. Barrier Pattern – Involves synchronizing multiple threads, requiring careful coordination.
4. [[Actor model]] – Requires message-passing and avoiding shared state, used in Akka and Erlang.
5. [[Proactor Pattern]] – A more complex version of [[Reactor|Reactor]], often requiring OS-level async I/O support.
