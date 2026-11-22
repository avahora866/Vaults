---
tags:
  - Concurrency
  - FLASHCARD
  - "#SECONDARY_BRANCH"
---
Synchronization is the **coordination of multiple threads or processes to safely access shared resources** without causing data corruption, inconsistencies, or race conditions. In a multithreaded program, multiple threads may try to read or write the same variable, data structure, or I/O resource at the same time. Without synchronization, these concurrent accesses can lead to unpredictable results.
# Mutual Exclusion

Mutual Exclusion is the concept that only **one thread can access a shared resource at a time**. This prevents **race conditions**, where multiple threads read and write to a resource at the same time leading to inconsistent results. Mutual exclusion is enforced through various synchronisation mechanisms like mutex and locks. They make sure that only one thread is in the critical section (using the shared resource) at a time

## Lock
A lock is synchronisation mechanism that enforces mutual exclusion. A **thread must acquire the lock before reaching the critical section** (part of the code that accesses a shared resource). When the lock has been acquired,  other threads who are trying the critical section are **blocked** (or spin, depending on the implementation). There are different types of locks such as:

- **Spinlock**: A simple lock where a thread continuously checks until the lock becomes available. Best for very short critical sections where blocking would be more expensive.
- **Mutex**: A mutual exclusion lock that blocks threads until they acquire it. Only the thread that locked it can unlock it, enforcing ownership.
- **Recursive/Reentrant lock**: A lock that allows the same thread to acquire it multiple times without deadlocking itself. Useful when a function holding a lock calls another function that also requires the same lock.
- **Read/Write lock (shared-exclusive lock)**: A lock that lets multiple threads read a resource at the same time, but only one thread write at a time. Optimized for workloads with many reads and fewer writes.
- **Ticket lock**: A fairness lock where threads take a ticket number and wait until their number is served. Ensures first-come-first-served ordering.
- **Queue-based lock (MCS/CLH)**: Locks where threads form a queue and each spins on its own variable. Reduces contention and cache-line bouncing compared to plain spinlocks.

# Semaphores
A semaphore is a synchronisation primitive (synchronisation building block) that **controls access to a shared resource**. The **counter** represents the number of available "**permits**". A thread wanting access to a shared resource performs a "**wait**" (or p operation) which **decrements the counter**. If the counter is **greater than 0** then the thread proceeds otherwise (counter is 0) the thread is blocked until another thread (who is using the shared resource) performs the "**signal**" (or V) operation which **increments the counter** and unblocks the thread

Semaphores come in two main forms:
- **Counting Semaphore**: Counter value is greater then one. Allows more than one thread to access a shared resource (database connection)
- **Binary Semaphore**: behaves similarly to a mutex in that its counter can only be 0 or 1, effectively allowing one thread at a time

The difference between locks and semaphores is **ownership and purpose**. A lock **only allows** the thread currently using the shared resource to **release the lock** whilst a semaphore **allows any thread** to perform a "signal" to **increment the counter**. Semaphores don't enforce strict ownership, **any thread** can **increment the counter** even if it had not decremented the counter before. Could lead to errors if used incorrectly

# Condition variables
Condition variables are synchronisation primitives used to let threads wait for specific conditions to become true. Used with a lock.
- A condition variables allows a thread to sleep (wait) until another thread signals that some shared state has changed
- It avoids busy-waiting (spinning in a loop checking a condition), which wastes CPU cycles

## How it works
1. A thread locks a mutex and checks a condition
2. If the condition is false, the thread calls `wait()` on the condition variable:
	- It releases the lock automatically
	- It goes to sleep until another thread signals the condition variable
	- When woken up it re-acquires the mutex before returning from `wait()`
3. Another thread changes the shared state and calls `signal()` or `broadcast()` on the condition variable to wake up one or all threads

```java
SharedResource resource = new SharedResource();

Thread waiter = new Thread(() -> {
    try {
        resource.waitForCondition();
    } catch (InterruptedException e) {
        Thread.currentThread().interrupt();
    }
});

Thread signaler = new Thread(resource::signalCondition);

waiter.start();
Thread.sleep(1000);
signaler.start();
```

*Note: A condition variable itself is not an actual logical condition like `inUse == true`. It is a mechanism that threads use to wait and signal when such a condition to use. The idea is that of a thread going to sleep when critical section is in use and waking up when the other thread calls notify() or broadcast() when the condition variable changes (i.e. critical section is now available)*

# Monitors and Monitor Variables
## Monitor
A monitor is a concurrency construct that combines:
- Mutual exclusion (only one thread can execute inside it at a time)
- Condition variables (for waiting and signalling between threads)
Its essentially a lock + one or more condition variables, packaged as a higher-level synchronisation mechanism

*Note: When using Java's `synchronised` keyword or a `ReentrentLock`, your effectively working with a monitor.*

### How it works
1. A thread enters the monitor by acquiring its lock
2. It checks a condition (predicate on shared state)
3. If the condition isn't satisfied, it waits on a condition variable inside the monitor:
	- This releases the monitor's lock
	- The thread sleeps until another thread signals the condition variable
4. When signalled, the thread wakes, re-acquires the lock, rechecks the condition and proceeds
5. When finished, it exits the monitor, releasing the lock

In summary, monitors are concurrency constructs that coordinate when threads sleep and wake based on the monitor variables and ensure mutual exclusion to restrict the access to the construct to 1 thread at a time.  

*Note: In java every object is a monitor. So the object is the lock. If an object has two methods in it that are both synchronised and thread A calls method A + thread B calls method B at the same time, despite calling different methods one of the threads will need to wait due to the actual object being the lock*

## Monitor Variables (Condition variables inside a monitor)
Monitor variables are simply condition variables that are inside a monitor. Usually an implicit `wait()` or `notify()` 

# Read/Write locks
A read/write lock is a synchronization primitive that allows for higher concurrency than a standard lock (like a mutex) in specific scenarios.

It provides two separate locks:
- Read Lock (Shared): Multiple threads can acquire the read lock simultaneously.
- Write Lock (Exclusive): Only one thread can acquire the write lock at a time.

## How It Works
The lock enforces these rules:
- Multiple Readers: If no thread holds the write lock, any number of threads can acquire the read lock. This allows many threads to read the shared data in parallel.
- Exclusive Writer: A thread can only acquire the write lock if no other thread holds either a read lock or the write lock.
- Writers Block Readers: If a thread holds the write lock, all other threads (both readers and writers) must wait until the writer releases the lock.
- Readers Block Writers: If any thread holds a read lock, a thread attempting to acquire the write lock must wait until all readers have released their locks.

## Primary Use Case
Use a read/write lock when you have a shared resource that is read very often but modified infrequently.

Example: An in-memory cache (like a Map) that stores configuration data. This data is read by many threads on almost every request, but it is only written (updated) very rarely, perhaps once every few hours.

# Deadlock
Deadlock is a state in concurrent programming where two or more processes are blocked forever, each waiting for a resource held by another process in the same set.
This creates a circular dependency where no process can proceed.

## The four conditions of deadlock
For a deadlock to occur, all four of these conditions, known as the Coffman conditions, must be true at the same time:

1. Mutual Exclusion: At least one resource must be held in a non-sharable mode. Only one process can use that resource at any given time. If another process requests it, that process must wait.
2. Hold and Wait: A process must be holding at least one resource while simultaneously waiting to acquire additional resources that are held by other processes.
3. No Preemption: Resources cannot be forcibly taken away from a process. A process must release its resources voluntarily after it has finished its task.
4. Circular Wait: A set of waiting processes $\{P_0, P_1, ..., P_n\}$ must exist such that $P_0$ is waiting for a resource held by $P_1$, $P_1$ is waiting for a resource held by $P_2$, ..., $P_n-1$ is waiting for a resource held by $P_n$, and $P_n$ is waiting for a resource held by $P_0$.

## Deadlock Detection
If the system does not use a prevention or avoidance strategy, it must be able to detect deadlocks and recover from them.

Resource Allocation Graph (RAG): This is the most common detection method. The system maintains a directed graph that tracks which process holds which resource and which process is waiting for which resource.
- Processes are represented as circles.
- Resource types are represented as squares.
- An arrow from a process to a resource means the process is requesting it.
- An arrow from a resource to a process means the resource is allocated to it.

Cycle Detection: The system periodically runs an algorithm to check for cycles in the graph.
- If the graph has no cycles, the system is not in a deadlocked state.
- If the graph has a cycle, a deadlock may exist. If each resource in the cycle has only one instance, a deadlock definitely exists.

## Deadlock Recovery
Once a deadlock is detected, the system must break it.

- Process Termination:
    - Abort All Deadlocked Processes: This is the simplest but most drastic solution. It breaks the cycle but discards all computations done by those processes.
    - Abort One Process at a Time: The system aborts one "victim" process in the cycle and checks if the deadlock is resolved. If not, it aborts another. Choosing the victim is difficult (e.t., lowest priority, least amount of work done).
- Resource Preemption:
    - The system forcibly takes a resource from one process (the victim) and gives it to another process in the cycle.
    - This is complex. You must choose a victim, safely stop its use of the resource, and roll back the victim process to a state where it can be safely restarted later. This can lead to starvation, where the same process is repeatedly chosen as the victim.

## Deadlock Prevention and Avoidence
These are proactive strategies to handle deadlock.

### Deadlock Prevnetion
This strategy ensures that at least one of the four necessary conditions can never occur.

- Break Mutual Exclusion: Make all resources sharable. This is often impossible (e.g., a printer cannot be used by two processes at once).
- Break Hold and Wait:
    - Require a process to request all its resources at the beginning. It cannot start until all are granted.
    - This is inefficient as it holds resources it may not need until much later.
- Break No Preemption:
    - If a process holding resources requests another resource and is denied, it must release all its held resources.
    - This is only practical for resources whose state can be easily saved and restored (e.g., CPU registers).
- Break Circular Wait:
    - Impose a total ordering on all resource types (e.g., $R_1, R_2, ..., R_n$)
    - Require that processes only request resources in increasing order. A process holding $R_i$ can request $R_j$, but only if $j > i$. This makes a circular wait impossible.

### Deadlock Avodience
This strategy allows all four conditions but uses algorithms to ensure the system never enters an unsafe state (a state that could lead to a deadlock).
This requires the system to know in advance the maximum number of resources each process might request.

- Safe State: A state is safe if there is some sequence in which all processes can run to completion, even if they all request their maximum resources.
- Banker's Algorithm: This is the classic avoidance algorithm.
    1. When a process enters the system, it must declare the maximum number of each resource it could ever need.
    2. When a process requests resources, the system checks if granting the request will leave the system in a safe state.
    3. The Check: The system pretends to grant the request. Then, it checks if there is at least one process that can run to completion with the remaining available resources. If so, it assumes that process finishes and releases its resources. The system repeats this check until all processes can finish.
    4. If a safe sequence exists, the request is granted.
    5. If granting the request leads to an unsafe state, the process must wait, even if the resources are technically available.