---
tags:
  - Theory
---
A time slice (also called a quantumn) is a property on a OS thread that is configured by the OS scheduler. It gives the maximum amout of time that the thread is allowed to execute on a CPU core before it might be paused for another thread. 

- On a **single-core CPU**, only one thread runs at a time. The OS scheduler gives each runnable thread a time slice so they all get a fair share of the CPU.
- On a **multi-core CPU**, each core still time-slices between the runnable threads assigned to it.

How it works:

1. Scheduler picks a thread from the ready queue.
2. That thread runs on the CPU for its time slice (say 10 ms).
3. If it finishes or blocks (like waiting on I/O), the scheduler can switch earlier.
4. If the time slice ends, a context switch occurs and another thread gets the CPU.