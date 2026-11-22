---
tags:
  - Concurrency
  - FLASHCARD
  - "#SECONDARY_BRANCH"
---
A race condition is when two or more threads try to access the same shared data and try to change it at the same time. The end result of the shared data is dependent on the timing of the operations.

1. Multiple threads read or write to the same shared variable or resource.
2. The operations interleave unpredictably (depending on CPU scheduling, context switching, etc.).
3. Because these operations are not atomic, a thread may read stale or partial data, leading to inconsistent or incorrect results.

```java
class Counter {
    private int count = 0;

    public void increment() {
        count++;  // Not atomic
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) counter.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) counter.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final count: " + counter.getCount());
    }
}
```
You might expect the output to be 2000, but it’s often less because:
- count++ is actually multiple steps:
- Read count
- Add 1
- Write back the result

If both threads read the same value before either writes back, one increment is lost.

# Methods for preventing race conditions
To prevent synchronisation use syncronisation mechnisms to make access atomic or ordered:
- Synchronised blocks => Link
- Locks => Link
- Atomic Variables: variable that provides atomic oeprations on its value

# Atomic operations
An atomic operation is one that cannot be interrupted or observed in an incomplete state. It either happens entirely or not at all
For example:
- Incrementing a variable (x = x + 1) is not atomic because it involves multiple steps: read, add, write.
- A true atomic increment would perform all of that as a single uninterruptible instruction.
In other words, atomic operations are indivisible and thread-safe by design.

## Atomic Variable
An atomic variable is a data type that provides atomic operations on its value.
It’s a programming construct that ensures operations like read, write, or increment are atomic without needing explicit locks.

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {
    private final AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();  // atomic operation
    }

    public int getCount() {
        return count.get();       // atomic read
    }
}
```
- AtomicInteger is the atomic variable.
- incrementAndGet() is the atomic operation it provides.

## Compare and Swap (CAS) operations
CAS allows for [[Lock|lock]]-free concurrency. It is used when you might want to concurrently modify the same data but don't need to block each other

CAS works by performing a read-modify-write operation in a single atomic step:

- **Read**: The thread reads the current value of the shared data.  
- **Modify**: The thread computes a new value based on the current value.  
- **Compare**: The thread checks whether the current value is still the same as when it was read (i.e., whether another thread has modified it in the meantime).  
- **Swap**: If the value hasn't changed (i.e., no other thread has modified it), the thread writes its new value back. If the value has changed, the thread retries the operation.

# Memory visability issues
Memory visibility issues occur when one thread updates a shared variable, but other threads don’t see the updated value immediately — or at all — because of how modern CPUs and compilers optimize memory access.

Modern systems use caches and compiler reordering to improve performance:
- Each thread may have its own CPU cache.
- The compiler and CPU can reorder instructions if it believes it won’t affect single-threaded logic.

In a multithreaded context, this leads to problems:
- Thread A updates a variable in its cache.
- Thread B reads the same variable but from its own stale cache.
- The write from A isn’t visible to B yet.

```java
class Shared {
    private boolean running = true;

    public void stop() {
        running = false;
    }

    public void runLoop() {
        while (running) {
            // do work
        }
        System.out.println("Stopped");
    }
}
```
If two threads do:
```java
Shared s = new Shared();
new Thread(s::runLoop).start();
Thread.sleep(100);
s.stop();
```
You might expect runLoop() to exit when stop() sets running = false.
But it might never exit, because:
- The running variable can be cached in the worker thread’s CPU core.
- That thread may never see the new value from main memory.

## Fixes across languages
| Language | Visibility Control                                     |
| -------- | ------------------------------------------------------ |
| Java     | `volatile`, `synchronized`                             |
| C++      | `std::atomic` (implies visibility via memory ordering) |
| C#       | `volatile`, `lock`                                     |
| Rust     | `Atomic*` types with memory order guarantees           |

# Barriers and Fences => Linked note
Barriers and fences are low-level synchronization mechanisms that control how the CPU and compiler order and make visible memory operations. They’re essential to prevent subtle concurrency bugs caused by instruction reordering or stale cache data.

## Memory Reordering 
Modern CPUs and compilers can reorder reads and writes to improve performance.

```java
// Thread A
x = 1;      // write 1
y = 1;      // write 2

// Thread B
if (y == 1)
    print(x);
```
You might assume that if Thread B sees y == 1, then it must also see x == 1.
but without a barrier or synchronization, the CPU might reorder the writes — so y becomes visible before x.

### Memory Barriers / fences
A barrier (or fence) is an instruction that forces ordering and visibility of memory operations.
It tells the CPU and compiler:
“All memory operations before this point must be completed and visible before anything after this point starts.”
Barriers prevent reordering, cache delays, and store/load optimizations that can break concurrency.
#### Types of barriers
| Type                              | What it prevents                       | Description                                                                 |
| --------------------------------- | -------------------------------------- | --------------------------------------------------------------------------- |
| **Load barrier (read barrier)**   | Reordering of reads before the barrier | Ensures all reads before are completed before later reads.                  |
| **Store barrier (write barrier)** | Reordering of writes after the barrier | Ensures all writes before are visible before later writes.                  |
| **Full barrier (memory fence)**   | Reordering of both reads and writes    | Ensures all memory operations before the barrier complete before any after. |

### Language-level abstractions
Most high-level languages don’t expose fences directly — they’re built into concurrency primitives.
| Language | Equivalent                                                                                                                      |
| -------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **Java** | Happens-before relationships (via `volatile`, `synchronized`, `Lock`, `Atomic*`) implicitly insert fences.                      |
| **C++**  | `std::atomic` with `memory_order_release`, `memory_order_acquire`, or `memory_order_seq_cst` explicitly define fence semantics. |
| **Rust** | Similar to C++ with atomic memory orderings (`Ordering::SeqCst`, `Acquire`, `Release`, etc.).                                   |
| **C**    | Uses compiler intrinsics like `__sync_synchronize()` or `atomic_thread_fence(memory_order_seq_cst)`                             |