---
tags:
  - "#TRUNK"
  - Java
  - Concurrency
---

# Java Concurrency & Multithreading

Java's concurrency model is one of its defining strengths for backend development — and one of its most dangerous surfaces if misunderstood. This note covers the complete concurrency curriculum from first principles (threads and shared state) through production-grade infrastructure (`ExecutorService`, `CompletableFuture`), with the goal of making you confident reasoning about concurrent Java systems under real load.

---

# 1. The Concurrency Problem — Why It Matters

Every backend system you build at scale will eventually confront a question that doesn't exist in single-threaded code: *what happens when two things try to change the same data at the same time?*

A running Java application is a single **process**. Processes are independent and do not share memory. Inside that process, however, you can spawn many **threads** — each an independent unit of execution. The critical fact is this: all threads within a process share the same **heap**. Each thread gets its own **stack** (for local variables and method frames), but heap objects — your service classes, collections, counters, any object you `new` into existence — are visible to every thread simultaneously.

This shared heap is the root cause of virtually every concurrency problem in Java. Code that works perfectly in isolation can behave incorrectly, intermittently and without warning, the moment a second thread touches the same object.

```
JVM Process
├── main thread         ← your code starts here (own stack)
├── GC thread           ← garbage collector (own stack)
├── JIT compiler thread ← HotSpot optimisation (own stack)
└── ... other daemon threads

Shared heap ← ALL threads read and write here
```

---

# 2. Threads, Runnable & the Thread Lifecycle

**Creating a thread is the entry point to the entire concurrency model**, so understanding the right way to do it matters immediately. Java offers three approaches.

## Approach 1 — Extending `Thread`

```java
public class PaymentProcessor extends Thread {
    private final Payment payment;

    public PaymentProcessor(Payment payment) {
        this.payment = payment;
    }

    @Override
    public void run() {
        System.out.println("Processing: " + payment.getId()
            + " on thread: " + Thread.currentThread().getName());
    }
}

PaymentProcessor processor = new PaymentProcessor(payment);
processor.start(); // ← start(), NOT run()
```

> ⚠️ **The Crucial Detail:** calling `run()` directly executes on the *current* thread — no new thread is created. Only `start()` spawns a new OS thread and invokes `run()` on it. This is an extremely common mistake.

## Approach 2 — Implementing `Runnable` (preferred)

```java
public class PaymentTask implements Runnable {
    private final Payment payment;

    public PaymentTask(Payment payment) {
        this.payment = payment;
    }

    @Override
    public void run() {
        System.out.println("Processing: " + payment.getId());
    }
}

Thread thread = new Thread(new PaymentTask(payment));
thread.start();
```

This is the preferred approach. By implementing `Runnable` rather than extending `Thread`, the task class retains its single inheritance slot for something meaningful. More importantly, it cleanly **separates the task from the execution mechanism** — a design principle that becomes critical when thread pools enter the picture.

## Approach 3 — Lambda (modern Java)

Since `Runnable` is a functional interface, a lambda works directly:

```java
Thread thread = new Thread(() -> {
    System.out.println("Processing on: " + Thread.currentThread().getName());
});
thread.start();
```

---

## The Thread Lifecycle

```
                  start()
NEW ─────────────────────────→ RUNNABLE
                                    │
                    ┌───────────────┼───────────────┐
                    ↓               ↓               ↓
                 BLOCKED         WAITING      TIMED_WAITING
               (lock attempt)   (wait())     (sleep(ms)/wait(ms))
                    │               │               │
                    └───────────────┴───────────────┘
                                    │ lock acquired / notify() / timeout
                                    ↓
                                RUNNABLE
                                    │ run() completes
                                    ↓
                               TERMINATED
```

| State | Meaning |
|---|---|
| `NEW` | Thread created, `start()` not yet called |
| `RUNNABLE` | Eligible to run or currently running — JVM scheduler decides |
| `BLOCKED` | Waiting to acquire a monitor lock |
| `WAITING` | Parked indefinitely — needs explicit `notify()` to resume |
| `TIMED_WAITING` | Waiting for a specified duration |
| `TERMINATED` | `run()` has completed |

`BLOCKED` and `WAITING` are distinct states. `BLOCKED` is specifically about monitor lock contention (`synchronized`). `WAITING` is about explicit coordination (`wait()` / `notify()`). Conflating them leads to incorrect diagnoses in production.

---

## Key Thread Methods

```java
Thread thread = new Thread(() -> processPayment());

thread.start();                        // create OS thread, invoke run()
thread.join();                         // calling thread waits for this one to terminate
thread.join(5000);                     // wait at most 5 seconds
Thread.sleep(1000);                    // pause current thread for 1 second
thread.setName("payment-processor-1"); // name threads — essential for debugging
thread.setDaemon(true);                // must be set BEFORE start()
thread.getState();                     // returns Thread.State enum
Thread.currentThread().getName();      // inspect the currently running thread
```

**Daemon threads** die automatically when all user threads complete — the JVM does not wait for them. Use daemon threads for background housekeeping (metrics, health checks) that should never prevent application shutdown.

---

## The Raw Thread Anti-Pattern

```java
// 10,000 concurrent users — catastrophic
for (Request request : incomingRequests) {
    new Thread(() -> handle(request)).start(); // ❌
}
```

Each OS thread consumes ~512KB–1MB of stack memory. At 10,000 threads, that is up to 10GB consumed just for stacks before a byte of application data is processed. The JVM throws `OutOfMemoryError: unable to create new native thread` well before that point. Thread pools are the solution — see Section 4.

---

## The Shared State Problem — A Preview

```java
public class Counter {
    private int count = 0;

    public void increment() {
        count++; // looks atomic — it is NOT
    }
}

Counter counter = new Counter();
Thread t1 = new Thread(() -> { for (int i = 0; i < 1000; i++) counter.increment(); });
Thread t2 = new Thread(() -> { for (int i = 0; i < 1000; i++) counter.increment(); });

t1.start(); t2.start();
t1.join();  t2.join();

System.out.println(counter.getCount());
// Expected: 2000. Actual: anywhere from ~1000 to 2000
```

`count++` compiles to three bytecode instructions: READ, INCREMENT, WRITE. Two threads can interleave these such that an increment from one thread is silently overwritten by the other. This is a **race condition** — non-deterministic, intermittent, and catastrophic in production.

---

# 3. Synchronization & Thread Safety

## The Monitor

Every Java object has an intrinsic lock called a **monitor**. Only one thread can hold a monitor at any given moment. The `synchronized` keyword is how you acquire it.

```java
public class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++; // only one thread executes this at a time
    }

    public synchronized int getCount() {
        return count;
    }
}
```

When Thread 1 enters `increment()`, it acquires the monitor on the `Counter` instance. Thread 2 is blocked (entering `BLOCKED` state) until Thread 1 releases the lock on exit. The monitor is tied to the *instance* — two separate `Counter` objects have two separate monitors and do not interfere.

---

## Four Forms of `synchronized`

**1. Synchronized instance method** — locks on `this`

```java
public synchronized void increment() {
    count++;
}
```

**2. Synchronized static method** — locks on the `Class` object, applies across all instances

```java
public static synchronized void logEvent(String event) {
    eventLog.add(event);
}
```

**3. Synchronized block on `this`** — preferred in most cases

```java
public void processPayment(Payment payment) {
    validatePayment(payment);       // concurrent — no lock needed
    enrichPaymentData(payment);     // concurrent

    synchronized (this) {
        accountBalance -= payment.getAmount(); // exclusive
    }

    sendConfirmationEmail(payment); // concurrent again
}
```

Synchronized blocks are preferable to synchronized methods because they minimise the **critical section** — the portion of code that must execute exclusively. Smaller critical sections mean less contention and higher throughput.

**4. Synchronized block on a dedicated lock object** — for fine-grained control

```java
public class OrderService {
    private final Object inventoryLock = new Object();
    private final Object balanceLock   = new Object();

    public void reserveItem() {
        synchronized (inventoryLock) { inventory--; }
    }

    public void deductBalance(double amount) {
        synchronized (balanceLock) { balance -= amount; }
    }
}
```

`reserveItem()` and `deductBalance()` can now execute concurrently — they use different monitors. Synchronizing both on `this` would serialize them unnecessarily, reducing throughput.

---

## What Synchronization Actually Guarantees

`synchronized` provides two guarantees that are frequently conflated:

**Mutual exclusion** — only one thread executes the critical section at a time.

**Memory visibility** — this is less understood but equally critical. Modern CPUs maintain per-core caches. A thread on Core 1 may hold a locally cached copy of a variable that differs from main memory. Without synchronization, a second thread on Core 2 may read a stale value indefinitely.

`synchronized` guarantees that when a thread *acquires* a lock it reads the latest values from main memory, and when it *releases* a lock it flushes all writes back to main memory.

```java
// Without synchronization — Thread 2 may loop forever
private boolean running = true;
// Thread 1: running = false;
// Thread 2: while (running) { } // may never see the update

// With synchronization — visibility guaranteed
private final Object lock = new Object();
// Thread 1: synchronized (lock) { running = false; }
// Thread 2: synchronized (lock) { while (running) { } } // guaranteed to see update
```

---

## `volatile` — Visibility Without Mutual Exclusion

For cases where you only need visibility — not atomicity — `volatile` is lighter than `synchronized`:

```java
private volatile boolean running = true;

// Thread 1: running = false;     — immediately flushed to main memory
// Thread 2: while (running) { }  — always reads from main memory
```

> ⚠️ **The Crucial Detail:** `volatile` guarantees visibility of the *variable itself* only. It does not make compound operations atomic. `volatile int count; count++` is still a race condition — READ, INCREMENT, WRITE are still three separate operations. Use `volatile` for simple flags and status indicators, never for counters or compound state.

---

## Deadlock

Two threads each holding a lock the other needs — neither can proceed:

```java
// Thread 1 calls transferAtoB(), Thread 2 calls transferBtoA() simultaneously
synchronized (lockA) {      // Thread 1 holds lockA
    synchronized (lockB) {  // Thread 1 waits for lockB — held by Thread 2
        performTransfer();
    }
}

synchronized (lockB) {      // Thread 2 holds lockB
    synchronized (lockA) {  // Thread 2 waits for lockA — held by Thread 1
        performTransfer();
    }
}
// Neither thread proceeds. Neither releases. Deadlock.
```

**Prevention — lock ordering.** Always acquire multiple locks in the same global order, eliminating the possibility of a circular wait:

```java
public void transfer(BankAccount target, double amount) {
    // Always lock the lower-ID account first, regardless of transfer direction
    BankAccount first  = this.accountId < target.accountId ? this : target;
    BankAccount second = this.accountId < target.accountId ? target : this;

    synchronized (first) {
        synchronized (second) {
            if (this.balance >= amount) {
                this.balance -= amount;
                target.balance += amount;
            }
        }
    }
}
```

Whether the transfer is A→B or B→A, the lower-ID account is always locked first. A circular wait becomes structurally impossible.

---

## Liveness Problems Beyond Deadlock

| Problem | Description |
|---|---|
| **Deadlock** | Two threads waiting on each other — neither can proceed |
| **Livelock** | Threads are active but making no progress — each keeps deferring to the other |
| **Starvation** | A thread is perpetually denied access because others always acquire the lock first |

Livelock is particularly insidious: threads appear active in monitoring tools but accomplish nothing. The classic example is two threads that each detect a conflict, simultaneously back off, then simultaneously retry — indefinitely.

---

## Synchronization Performance Cost

An uncontended lock is cheap — the JVM optimises aggressively with lock elision. A contended lock is expensive: threads must be suspended and rescheduled by the OS. In a backend service where 100 threads synchronize on the same object, those threads effectively become serial — all the overhead of threading with none of the parallelism benefit. The mitigations are: minimize critical section size, use fine-grained dedicated lock objects, and for high-throughput paths, move to the lock-free structures in `java.util.concurrent`.

---

# 4. Executors & Thread Pools

## Why Thread Pools

```java
// Naive approach — catastrophic at scale
for (Order order : orders) {
    new Thread(() -> processOrder(order)).start(); // ❌
}
```

Thread creation is expensive (~1MB stack per OS thread). 10,000 tasks create 10,000 threads with no control over concurrency and no mechanism to retrieve results. The pool pattern solves all of these.

```java
ExecutorService executor = Executors.newFixedThreadPool(10);
executor.submit(() -> processOrder(order));
executor.shutdown();
```

You submit tasks; the pool assigns them to available threads. If all threads are busy, tasks queue. Threads are created once and reused — the creation cost is paid once.

---

## The Four Standard Pools

**Fixed Thread Pool** — for CPU-bound work

```java
ExecutorService executor = Executors.newFixedThreadPool(
    Runtime.getRuntime().availableProcessors()
);
```

Always exactly N threads. Excess tasks queue in an unbounded `LinkedBlockingQueue`. For CPU-bound work, the optimal size is approximately the number of CPU cores — more threads than cores means context switching with no parallelism gain.

**Cached Thread Pool** — for bursty, short-lived I/O tasks

```java
ExecutorService executor = Executors.newCachedThreadPool();
```

Creates threads on demand, reuses idle ones, terminates threads idle for 60 seconds. No queue — tasks execute immediately.

> ⚠️ Dangerous under sustained load. A cached pool creates unbounded threads under pressure — the same problem as raw thread creation. Prefer fixed pools in backend services where load is unpredictable.

**Single Thread Executor** — for ordered sequential processing

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
```

One thread, tasks execute in submission order. If the thread dies, a replacement is created automatically. Useful for audit logging and ordered event processing.

**Scheduled Thread Pool** — for periodic and delayed tasks

```java
ScheduledExecutorService executor = Executors.newScheduledThreadPool(5);

executor.schedule(() -> sendReminder(), 10, TimeUnit.SECONDS);
executor.scheduleAtFixedRate(() -> syncCache(), 0, 60, TimeUnit.SECONDS);
executor.scheduleWithFixedDelay(() -> pollDatabase(), 0, 60, TimeUnit.SECONDS);
```

`scheduleAtFixedRate` fires every N seconds regardless of execution duration. `scheduleWithFixedDelay` waits N seconds *after the previous execution completes* — use this when task duration is variable and overlap would be dangerous.

---

## `execute()` vs `submit()` and `Future<T>`

```java
// execute() — fire and forget, no return value
executor.execute(() -> processOrder(order));

// submit() — returns a Future representing the pending result
Future<Double> future = executor.submit(() -> calculateRisk(portfolio));

Double result = future.get();                     // blocks until complete
Double result = future.get(5, TimeUnit.SECONDS);  // blocks at most 5 seconds
```

`Future<T>` represents a pending computation. It has real limitations that motivated `CompletableFuture`:

| Limitation | Description |
|---|---|
| `get()` blocks | The calling thread is suspended waiting for the result |
| No chaining | Cannot express "when this completes, do that" |
| No combining | Cannot easily wait for multiple futures |
| No callbacks | Cannot register a function to run on completion |

---

## `ThreadPoolExecutor` — Full Control

The factory methods hide important configuration. Production systems often need precise tuning:

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    10,                            // corePoolSize — threads always alive
    20,                            // maximumPoolSize — max under load
    60L, TimeUnit.SECONDS,         // idle time before extra threads die
    new ArrayBlockingQueue<>(500), // bounded task queue — critical
    new ThreadPoolExecutor.CallerRunsPolicy()
);
```

**The bounded queue is essential.** An unbounded `LinkedBlockingQueue` allows millions of tasks to queue during a traffic spike, consuming heap until `OutOfMemoryError`. A bounded `ArrayBlockingQueue` caps the queue; when full, the rejection policy activates.

**The four rejection policies:**

| Policy | Behaviour |
|---|---|
| `AbortPolicy` (default) | Throws `RejectedExecutionException` |
| `CallerRunsPolicy` | The calling thread executes the task itself |
| `DiscardPolicy` | Silently drops the task |
| `DiscardOldestPolicy` | Drops the oldest queued task, retries submission |

`CallerRunsPolicy` is the most valuable for backend services. When the pool is saturated, the HTTP request thread processes the task itself — slowing down incoming requests and creating natural **backpressure**. The system degrades gracefully rather than throwing exceptions or silently losing work. The bounded queue and `CallerRunsPolicy` work as a pair: the queue absorbs burst traffic, the policy handles overflow.

---

## `invokeAll()` and `invokeAny()`

```java
List<Callable<Report>> tasks = reportIds.stream()
    .map(id -> (Callable<Report>) () -> generateReport(id))
    .collect(Collectors.toList());

// Blocks until ALL tasks complete
List<Future<Report>> results = executor.invokeAll(tasks);

// Returns the FIRST successful result, cancels the rest
Report first = executor.invokeAny(tasks);
```

`invokeAny()` is powerful for redundant execution — submit the same query to multiple data sources and take whichever responds first.

---

## Proper Shutdown

```java
// Production-grade graceful shutdown
executor.shutdown();
try {
    if (!executor.awaitTermination(30, TimeUnit.SECONDS)) {
        executor.shutdownNow(); // force-cancel remaining tasks
    }
} catch (InterruptedException e) {
    executor.shutdownNow();
    Thread.currentThread().interrupt(); // restore interrupted status
}
```

> ⚠️ **Never skip shutdown.** Threads in a pool are user threads — a JVM with an active thread pool will not exit even when `main()` returns. Failure to shut down thread pools is a common source of application hangs during deployment.

---

## Putting It Together — The Fixed `ReportService`

The broken service from Section 2 (500 raw threads, unsafe `ArrayList`, no join) resolved:

```java
public class ReportService {
    private final ExecutorService executor =
        Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());

    public List<String> generateReports(List<String> reportIds)
            throws InterruptedException, ExecutionException {

        List<Callable<String>> tasks = reportIds.stream()
            .map(id -> (Callable<String>) () -> buildReport(id))
            .collect(Collectors.toList());

        List<Future<String>> futures = executor.invokeAll(tasks); // blocks until ALL done

        List<String> results = new ArrayList<>();
        for (Future<String> f : futures) results.add(f.get());
        return results;
    }
}
```

All three original problems resolved: bounded thread count, guaranteed completion before returning, and no unsafe shared collection manipulation.

---

# 5. CompletableFuture & Async Pipelines

`Future<T>`'s blocking `get()` means a thread waiting on a result is doing nothing — consuming memory and a pool slot while parked. In a backend processing thousands of concurrent requests, blocked threads are wasted threads. `CompletableFuture` eliminates this entirely.

`CompletableFuture<T>` implements both `Future<T>` and `CompletionStage<T>`. The latter is the key addition: a computation that can be *chained*, *combined*, and *subscribed to* without blocking.

---

## Creating a `CompletableFuture`

```java
// supplyAsync — async computation that returns a value
CompletableFuture<OrderResult> future =
    CompletableFuture.supplyAsync(() -> processOrder(order), executor);

// runAsync — async computation with no return value
CompletableFuture<Void> future =
    CompletableFuture.runAsync(() -> sendNotification(order), executor);
```

> ⚠️ **Always provide your own executor.** The default `ForkJoinPool.commonPool()` is shared across the entire application — framework internals, parallel streams, third-party libraries. Exhausting it blocks unrelated operations unpredictably.

---

## Chaining

Rather than blocking for a result and then processing it, you declare what should happen *when* the result arrives.

**`thenApply()`** — transform the result into a new value

```java
CompletableFuture<OrderConfirmation> pipeline =
    CompletableFuture.supplyAsync(() -> processOrder(order), executor)
        .thenApply(result -> applyDiscount(result))
        .thenApply(result -> generateConfirmation(result));
```

**`thenAccept()`** — consume the result with no return value

```java
CompletableFuture.supplyAsync(() -> processOrder(order), executor)
    .thenAccept(result -> sendConfirmationEmail(result));
```

**`thenRun()`** — execute after completion, ignoring the result entirely

```java
CompletableFuture.supplyAsync(() -> processOrder(order), executor)
    .thenRun(() -> updateMetrics());
```

Each chaining method has an `Async` variant (`thenApplyAsync`, etc.) that runs the next stage on the thread pool rather than on the completing thread. Use async variants for heavy subsequent computations; use plain variants for lightweight transformations.

---

## Combining Futures — True Parallelism

**`thenCombine()`** — two independent futures in parallel, combine both results

```java
CompletableFuture<UserProfile> profileFuture =
    CompletableFuture.supplyAsync(() -> fetchUserProfile(userId), executor);

CompletableFuture<CreditScore> creditFuture =
    CompletableFuture.supplyAsync(() -> fetchCreditScore(userId), executor);

CompletableFuture<LoanApplication> application =
    profileFuture.thenCombine(creditFuture,
        (profile, credit) -> buildLoanApplication(profile, credit));
```

Without `CompletableFuture`, these calls would be sequential — fetch profile, block, fetch credit score, block. This pattern executes both simultaneously and combines when both are ready.

**`allOf()`** — wait for all futures to complete

```java
List<CompletableFuture<Report>> reportFutures = reportIds.stream()
    .map(id -> CompletableFuture.supplyAsync(() -> generateReport(id), executor))
    .collect(Collectors.toList());

CompletableFuture.allOf(reportFutures.toArray(new CompletableFuture[0]))
    .thenRun(() -> {
        List<Report> reports = reportFutures.stream()
            .map(CompletableFuture::join) // safe — all already complete
            .collect(Collectors.toList());
        publishReports(reports);
    });
```

**`anyOf()`** — complete when the first future completes

```java
CompletableFuture<Object> fastest = CompletableFuture.anyOf(
    CompletableFuture.supplyAsync(() -> queryPrimaryDb(), executor),
    CompletableFuture.supplyAsync(() -> queryReplicaDb(), executor),
    CompletableFuture.supplyAsync(() -> queryCache(), executor)
);
fastest.thenAccept(result -> sendResponse(result));
```

---

## `thenCompose()` vs `thenApply()` — A Critical Distinction

```java
// thenApply — next stage returns a plain value T
CompletableFuture<Report> future =
    CompletableFuture.supplyAsync(() -> fetchData())
        .thenApply(data -> buildReport(data)); // buildReport returns Report

// thenCompose — next stage returns CompletableFuture<T>
CompletableFuture<Report> future =
    CompletableFuture.supplyAsync(() -> fetchData())
        .thenCompose(data -> generateReportAsync(data)); // returns CompletableFuture<Report>
```

`thenCompose()` is the `flatMap` of `CompletableFuture` — it flattens `CompletableFuture<CompletableFuture<T>>` into `CompletableFuture<T>`. Use it whenever the next stage is itself an async operation returning a `CompletableFuture`; otherwise you end up with a nested future that doesn't compose.

---

## Error Handling

**`exceptionally()`** — recover from failure with a fallback value

```java
CompletableFuture.supplyAsync(() -> fetchUserData(userId), executor)
    .exceptionally(throwable -> {
        log.error("Failed to fetch user data", throwable);
        return UserData.defaultProfile(); // pipeline continues with fallback
    })
    .thenAccept(data -> renderProfile(data));
```

**`handle()`** — handle both success and failure in one stage

```java
CompletableFuture.supplyAsync(() -> processPayment(payment), executor)
    .handle((result, throwable) -> {
        if (throwable != null) return PaymentResult.failed(throwable.getMessage());
        return result;
    })
    .thenAccept(result -> notifyClient(result));
```

`handle()` executes regardless of whether the previous stage succeeded or failed — use it when you need to always produce a result. `exceptionally()` is simpler for failure-only fallbacks.

---

## A Complete Real-World Pipeline

A flight booking service: validate passport, then check seat availability and payment in parallel, then confirm:

```java
public CompletableFuture<BookingResult> confirmBooking(Passenger passenger,
                                                        Booking booking) {
    return CompletableFuture
        // Step 1 — validate passport
        .supplyAsync(() -> validatePassport(passenger), executor)

        // Step 2 — check seat and payment IN PARALLEL
        .thenCompose(passportResult -> {
            CompletableFuture<SeatResult> seatFuture =
                CompletableFuture.supplyAsync(() -> checkSeatAvailability(booking), executor);
            CompletableFuture<PaymentResult> paymentFuture =
                CompletableFuture.supplyAsync(() -> validatePayment(passenger), executor);

            return seatFuture.thenCombine(paymentFuture,
                (seat, payment) -> new BookingContext(seat, payment));
        })

        // Step 3 — confirm and charge
        .thenApply(bookingContext -> confirmAndCharge(bookingContext))

        // Step 4 — handle any failure in the entire pipeline
        .exceptionally(throwable -> BookingResult.failed(throwable.getMessage()));
}
```

| Step | Method | Reason |
|---|---|---|
| Validate passport | `supplyAsync` | Start async computation returning a value |
| Chain to parallel checks | `thenCompose` | Next stage itself returns a `CompletableFuture` |
| Seat + payment in parallel | `thenCombine` | Two independent futures, both results needed |
| Confirm and charge | `thenApply` | Transform the combined result into a new value |
| Handle failures | `exceptionally` | Failure-only fallback covering the entire pipeline |

**Why this beats sequential blocking at scale:** a blocking implementation would validate the passport (~200ms), check seats (~150ms), validate payment (~150ms), confirm (~100ms) — ~600ms total, a thread suspended the entire time. This pipeline runs seat and payment checks in parallel (~450ms wall-clock time), and no thread is ever blocked during I/O — only occupied during real computation. At 10,000 concurrent requests, the difference between a captive thread pool and a non-blocking pipeline is the difference between a system that degrades gracefully and one that falls over.

---

## `CompletableFuture` Method Reference

| Method | Purpose |
|---|---|
| `supplyAsync()` | Start async computation returning a value |
| `runAsync()` | Start async computation with no return |
| `thenApply()` | Transform result synchronously |
| `thenApplyAsync()` | Transform result on thread pool |
| `thenAccept()` | Consume result, no return |
| `thenRun()` | Execute after completion, ignores result |
| `thenCombine()` | Combine two independent futures |
| `thenCompose()` | Chain with another async operation — flatMap |
| `allOf()` | Wait for all futures to complete |
| `anyOf()` | Complete when first future completes |
| `exceptionally()` | Recover from failure with fallback |
| `handle()` | Handle both success and failure |
