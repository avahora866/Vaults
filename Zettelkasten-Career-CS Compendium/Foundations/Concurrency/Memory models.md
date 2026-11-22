---
tags:
  - Concurrency
  - FLASHCARD
---
A memory model is a set of rules that defines how threads interact with a computer's memory. It specifies the conditions under which a read operation in one thread is guaranteed to see the results of a write operation from another thread.

In short, a memory model is a contract between the hardware, the compiler, and your code that defines the rules for memory access in a multi-threaded program.

The JMM is an example of a memory model

# Why Memory Models Are Necessary
Without a memory model, concurrent programming would be extremely difficult. Modern systems perform complex optimizations to improve performance, but these optimizations break the simple, sequential logic that programmers expect.

- Compiler Optimizations: The compiler can reorder your instructions. If it sees two independent lines of code, it might execute them in a different order than you wrote them.
- CPU Optimizations: The processor itself executes instructions out-of-order to maximize throughput.
- Caching: Each CPU core has its own local cache. When Thread A (on Core 1) writes to a variable, that change might only exist in Core 1's local cache. Thread B (on Core 2) may read an old, stale value from main memory or its own cache.

A memory model provides the guarantees you, the programmer, need to manage these optimizations. It answers the question: "If Thread A writes to variable x, when is Thread B guaranteed to see that write?"

A memory model is a set of rules that defines how threads interact with a computer's memory. It specifies the conditions under which a read operation in one thread is guaranteed to see the results of a write operation from another thread.


In short, a memory model is a contract between the hardware, the compiler, and your code that defines the rules for memory access in a multi-threaded program.

Why Memory Models Are Necessary
Without a memory model, concurrent programming would be extremely difficult. Modern systems perform complex optimizations to improve performance, but these optimizations break the simple, sequential logic that programmers expect.


Compiler Optimizations: The compiler can reorder your instructions. If it sees two independent lines of code, it might execute them in a different order than you wrote them.

CPU Optimizations: The processor itself executes instructions out-of-order to maximize throughput.

Caching: Each CPU core has its own local cache. When Thread A (on Core 1) writes to a variable, that change might only exist in Core 1's local cache. Thread B (on Core 2) may read an old, stale value from main memory or its own cache.

A memory model provides the guarantees you, the programmer, need to manage these optimizations. It answers the question: "If Thread A writes to variable x, when is Thread B guaranteed to see that write?"

# Core Concepts
A memory model provides guarantees for three key concepts:

- Atomicity: An operation is atomic if it appears to all other threads as if it happened instantaneously. It cannot be seen as "half-done." For example, a write to a 64-bit long on a 32-bit system might not be atomic (it could require two separate writes). Non-atomic operations can lead to "word tearing," where one thread sees half of the old value and half of the new value.
- Visibility: This is the guarantee that when one thread writes to a shared variable, other threads will eventually see that new value. Without synchronization, a write by one thread might never become visible to another thread.
- Ordering: This defines the constraints on how operations can be reordered by the compiler or CPU. A memory model specifies which types of reordering are allowed and which are forbidden, and provides mechanisms (like memory barriers or fences) to enforce a specific order when needed.

# String vs Weak memory models
Memory models exist on a spectrum from "strong" to "weak."

## Strong Memory Model
- Provides more guarantees by default.
- Operations are generally visible to all threads in a clear, sequential order.
- This model is easier for programmers to reason about.
- The classic example is Sequential Consistency, where all operations from all threads appear to execute in a single global order, and the operations within each thread appear in the order specified by the program.
- Example: x86-64 processors have a relatively strong memory model.

## Weak (or Relaxed) Memory Model
- Provides fewer guarantees by default.
- Allows for more aggressive compiler and hardware optimizations (like reordering) to achieve higher performance.
- Operations from one thread may not be immediately visible to other threads, and their order may appear different to different threads.
- Programmers must explicitly use synchronization primitives (like locks, volatile, or atomics) to enforce visibility and ordering when needed.
- Example: ARM and POWER processors have weak memory models.