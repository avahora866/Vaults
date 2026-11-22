---
tags:
  - Concurrency
  - "#SECONDARY_BRANCH"
---
# Thread creation and management
Thread creation is the process of starting a new thread of execution within a program so that tasks can run concurrently. At its core, creating a thread means telling the runtime and operating system to set up:

- A separate [[Theory/OS/Stack|stack]] for the new thread
- A [[Program Counter|program counter]] pointing to the first instruction the thread will execute
- A set of **registers and scheduling metadata** so the OS can manage it independently of other threads

This means that when you create a thread in a programming language. During runtime the [[Process|process]] that your application runs on will have a new [[Thread|thread]] created inside it

From the programmer’s perspective, thread creation usually involves three steps:

1. **Define the work the thread will perform.**  
    This is often done by providing a function, runnable object, or lambda that contains the code the thread should execute.
2. **Start the thread.**  
    The program calls an API (like `new Thread(...).start()` in Java, `std::thread` in C++, or `Thread(target=...)` in Python) to ask the runtime/OS to create the new thread and assign it the task.
3. **Manage thread lifetime.**  
    Once started, the thread runs independently. The program can either wait for it to finish (joining) or let it run in the background. Threads usually terminate when their assigned function completes.

Under the hood, the operating system allocates resources for the thread (stack memory, thread control block) and registers it with the [[OS Scheduler|scheduler]]. The OS scheduler then decides when and on which CPU core the thread runs.

Thread creation is relatively expensive compared to reusing threads (as in thread pools), because it involves OS-level resource allocation and scheduling setup. This is why high-performance systems often prefer managing a fixed number of threads and submitting tasks to them instead of creating threads repeatedly.

Between creation and termination, a thread can be in different **situations that affect how the OS or runtime manages it**. These situations arise naturally from concurrency:

- A thread may be **ready** to run but waiting its turn because CPUs are limited.
- It may be **blocked** waiting for I/O, a lock, or another resource.
- It may have **finished execution**, but the OS still has to clean up its resources.

Different programming languages have different thread states depending on their level of abstract and detail they provide but they mostly map to the OS [[Thread|thread lifecycle stages]]:
- **Created/New** – Thread object exists, but hasn’t started.
- **Runnable/Ready** – Eligible to run, waiting for the CPU.
- **Running** – Currently executing instructions.
- **Blocked/Waiting** – Waiting for I/O, synchronization, or a signal.
- **Terminated/Dead** – Finished execution.

The programming language implementations of threads are as follows:
- [[Threads#Lifecycle of a Thread in Java|Java Thread Implementation]]
# Thread pooling
Thread pooling is a way of **reusing a fixed set of threads** to handle multiple tasks, instead of creating and destroying a new thread every time you need work done.

The concept exists because creating a thread is expensive:
- The OS must allocate memory for its stack, initialize control structures, and register it with the scheduler.
- Destroying a thread requires cleanup and resource reclamation. If your program creates thousands of short-lived threads, most of the CPU time gets wasted on overhead rather than actual work.

A **thread pool** avoids this by:
- Pre-allocating a pool of worker threads (say, 10 threads).
- Keeping those threads alive and waiting for tasks.
- When you submit a task, the pool assigns it to an available worker thread.
- Once the task finishes, the thread returns to the pool to be reused.
# Thread scheduling
[[OS Scheduler|Thread scheduling]] is the **mechanism by which the operating system (or runtime) decides which thread runs on the CPU at a given time**. The concept exists because most programs have more threads than CPU cores, so the system must share CPU time efficiently and fairly.

The main goals of thread scheduling are:
- **Maximize CPU utilization** – keep the CPU busy as much as possible.
- **Ensure responsiveness** – high-priority or interactive threads should get CPU quickly.
- **Provide fairness** – prevent starvation so all threads eventually run.

Scheduling is necessary because:
- Threads may block waiting for I/O or synchronization primitives.
- Threads may have different priorities or resource needs.
- Multiple threads compete for limited CPU cores.
    
Two broad categories of scheduling:
- **Preemptive** – the OS can interrupt a running thread to give another thread a chance to run. Most modern OSes use pre-emptive scheduling.
- **Cooperative** – threads yield control voluntarily; the OS relies on threads to give up the CPU. Rare in modern general-purpose systems but used in some runtimes or green-thread implementations.

Schedulers often use policies like **round-robin, priority-based, or multilevel feedback queues** to determine the order and duration of CPU allocation.

In high-level languages like Java or C#, the runtime maps language threads to OS threads, and scheduling decisions happen at the OS level.
# Thread-local storage
Thread-local storage (TLS) is a mechanism that allows each thread in a program to have its **own separate instance of a variable**. The concept exists to give threads **private storage** without interfering with other threads, while still sharing the same process memory.

This is different from the concept of each thread having its own **stack**. The thread stack holds local variables and function call frames which **only exist when the thread is executing** and is destroyed once the thread has finished. 

**Thread Local Storage (TLS)** lets you declare **global or static variables** that are **separate** for each thread. TLS variables persist **for the lifetime of the thread** and each thread can store and access its own **independent value** without affecting other threads, allowing threads to have **private state** in shared memory space **without synchronization**, unlike regular global or static variables.

The short summary is that a TLS variable is the same as a threads stack variable but it **can be static or global** and **constrained to that specific thread**. When you create a static variable in a **single threaded program** the **variable is shared across the program**. In a **multi-threaded program** the static variable can be access **across all threads** but any **TLS variables** have their **own instance** for each thread. We cant write the TLS variable as static as it will be shared across all threads then

The need arises because:
- In multithreaded programs, many threads might need to access some data simultaneously.
- If all threads share a single variable, you risk **race conditions** unless you synchronize access.
- Synchronization (locks, mutexes) can be expensive and reduce performance.

TLS solves this by letting each thread maintain its own copy of the variable:
- Threads can read and write to their copy **without locks**.
- The variable is automatically cleaned up when the thread exits.
- Each thread sees only its own value; other threads’ values are invisible.

Common uses:
- Caching data that is expensive to recompute per thread (e.g., formatting objects, database connections).
- Storing per-thread context like user sessions in server applications.
- Avoiding contention for frequently accessed variables in high-performance code.

Implementation examples:
- **Java:** `ThreadLocal<T>` provides TLS objects that each thread accesses independently.
- **C/C++:** `__thread` or `thread_local` keyword defines thread-local variables.
- **Python:** `threading.local()` provides per-thread storage.
