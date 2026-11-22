---
tags:
  - Concurrency
  - FLASHCARD
---
**Atomicity** means treating an operation or group of operations as a **single, indivisible unit**. Either the entire operation happens, or none of it happens — there’s no in-between state that other threads or processes can observe. This concept is critical in **concurrency** and **transaction systems**, because without atomicity, partial results could leak into shared state and corrupt correctness.

One example of this is [[Transactions]]

---
# Properties of Atomicity

1. **Indivisibility**: No other thread can see or affect the operation while it is being performed.
2. **Isolation**: The operation appears to take place **instantaneously** from the perspective of all threads.
3. **Consistency**: The shared data remains in a consistent state even under concurrent access.

---

# Hardware and Atomicity

* Most modern CPUs provide **atomic instructions** at the hardware level (like `LOCK` prefix on x86).
* These instructions ensure that atomic operations are **thread-safe at the processor level**.
