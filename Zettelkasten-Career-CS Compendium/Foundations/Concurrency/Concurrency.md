---
tags:
  - Concurrency
  - FLASHCARD
  - "#TRUNK"
---
Concurrency is when you structure your program to make progress on multiple tasks at the same time. 
At a hardware level programs are executed by **OS threads**. These threads are executed on the CPU core. On **single core machines** the concurrent program is executed through **context switching** (quickly switching between the different threads of your concurrent code), whilst on **multi-core machines** the concurrent code is executed in **parallel (parallelism).** 

# Context Switching
Context switching is what allows a single CPU core handle multiple tasks so that they appear to run at the "same time".

Step by step:
- A **thread** or **process** is running on the CPU.
- At some point, the OS scheduler decides to pause it (either because its time slice expired, or it’s waiting on I/O).
- The CPU’s state for that task (registers, program counter, stack pointer, etc.) gets saved into memory.
- The state of another thread/process is restored from memory into the CPU registers.
- Execution continues with this new task.

The actual switching between the tasks is called context switching.

Key points:
- It’s fast, but not free — saving and restoring registers, caches, and memory mappings takes time.
- Too many switches can hurt performance (this is called “context-switch overhead”).
- It gives the illusion of concurrency on a single-core machine.

# Thread vs Process
The [[Thread]] and [[Process]] are both units of execution at the OS level, however they differ in scope.

A **process** is an independent program in execution. Each process has its own private memory space that includes its **code, data, heap, and stack**. Because of this isolation, one process cannot directly access the memory of another. The OS assigns resources such as CPU time, memory, and file handles separately to each process. Creating a process is relatively expensive because the OS must allocate and manage these resources. Communication between processes requires special mechanisms like **pipes**, **sockets**, or **shared memory**, which are slower and more complex to implement. If one process crashes, it usually does not affect other processes.

A **thread**, on the other hand, is a smaller unit of execution that **lives inside a process**. Threads **share the process’s memory and resources**, **including its code and heap**, but each thread has its **own stack and program counter**. This makes **thread creation cheaper and faster** compared to processes. Since threads **operate in the same memory space, they can communicate with each other easily by reading and writing shared variables**. However, this also introduces risks like **race conditions** if synchronization is not handled correctly. If **one thread crashes**, it can bring down the **entire process** since they all share the same address space.
# Key concepts in concurrency
- [[Atomicity]] – Atomic operations prevent partial results from being observed, which is critical in concurrent programming to avoid corrupted state. For example, incrementing a counter must be atomic so two threads don’t overwrite each other’s updates.
- [[Non-blocking]] – Non-blocking algorithms avoid forcing one thread to wait while another holds a lock. This reduces contention and can improve responsiveness in systems with many concurrent tasks.
- [[Lock-freedom]] – Lock-free structures guarantee that even if one thread pauses or fails, other threads can still complete operations. This makes them resilient against thread stalls and deadlocks.
- [[Wait-freedom]] – Wait-free algorithms guarantee that every thread completes its operation in a finite number of steps, ensuring fairness and bounded latency regardless of scheduling.
- [[Obstruction-freedom]] – Obstruction-free designs allow a thread to make progress as long as it runs in isolation, but unlike lock-free or wait-free, they don’t guarantee global progress if contention is high.
- [[Consistency]] – Consistency ensures that updates to shared data always leave the system in a valid state. In databases, this might mean constraints are never violated; in memory, it might mean invariants always hold.
- [[Isolation]] – Isolation prevents concurrent tasks from seeing each other’s intermediate states. This helps avoid anomalies like dirty reads or lost updates, and it’s key in both transactions and multithreaded systems.
- [[Determinism vs. Non-determinism]] – Deterministic concurrency means the same input always produces the same output, regardless of scheduling. Non-deterministic concurrency allows different outcomes depending on timing, which can complicate debugging.
- [[Fairness]] – Fairness ensures no thread or task is indefinitely delayed, avoiding starvation. Scheduling policies like round-robin aim to provide fairness in practice.
- [[Idempotence]] – Idempotent operations can be retried safely without changing the result beyond the first execution. This property is essential in fault-tolerant systems that may reapply operations after crashes or timeouts.