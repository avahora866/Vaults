---
tags:
  - "#TRUNK"
  - Java
  - Backend
  - Enterprise
---

# Java — Backend & Enterprise

Java is a statically typed, object-oriented language that compiles to platform-independent bytecode executed by the Java Virtual Machine (JVM). It is the dominant language for enterprise backend systems — valued for its combination of strong type safety, mature ecosystem, and exceptional sustained throughput in long-running server processes. This trunk covers the Java Foundation, Core Backend Architecture, and Production-Grade practices for an advanced developer with prior experience in Python, C++, and JavaScript.

---

# 1. The JVM, JDK & JRE

Understanding Java's execution model is foundational — it explains both why Java is portable and why it performs the way it does in production.

## The Three-Layer Architecture

Java took a third path between C++ (compile to native per-platform) and Python (interpret source at runtime):

```
Your Java Source Code (.java)
        ↓
   [ javac compiler ]
        ↓
   Bytecode (.class)        ← platform-independent
        ↓
[ JVM for Windows ]
[ JVM for Linux   ]         ← platform-specific
[ JVM for macOS   ]
        ↓
  Machine Code (at runtime)
```

The `.class` bytecode is the unit of distribution — not a native binary. It is inert without a JVM to execute it.

## JDK, JRE, and JVM — Roles

| Component | Role | Who Needs It |
|-----------|------|--------------|
| **JDK** (Java Development Kit) | Full toolkit: `javac`, `java`, debugger, profilers, JRE | Developers |
| **JRE** (Java Runtime Environment) | JVM + standard class libraries — runs pre-built programs | End users (historically) |
| **JVM** (Java Virtual Machine) | Loads, verifies, and executes bytecode | Both |

> ⚠️ **Note:** As of Java 11, the JRE is no longer distributed separately. The JDK is the standard distribution.

JVM version compatibility is a real operational concern — bytecode compiled with JDK 21 may not run on a JRE 11 runtime. In enterprise deployments, version alignment between build environment and production runtime must be explicitly managed.

## The JIT Compiler — How Java Gets Its Performance

The JVM contains a **Just-In-Time (JIT) Compiler** (specifically HotSpot in Oracle/OpenJDK). Rather than interpreting every instruction naively, the JVM monitors execution and identifies frequently-run code paths — "hot spots". These are compiled to native machine code at runtime and cached.

Runtime behaviour:
1. JVM interprets bytecode initially
2. Monitors execution frequency per code path
3. Compiles hot paths → native machine code
4. All subsequent calls to those paths execute at near-native speed

| Runtime | Startup | Peak Throughput |
|---------|---------|-----------------|
| CPython | ~50ms | Moderate |
| Java (cold) | ~200–500ms | Very high (post-warmup) |
| Java (JIT warmed) | — | Comparable to C++ in many benchmarks |
| Native C++ | ~5ms | Highest |

This is why Java dominates enterprise backends — you trade startup time (acceptable for long-running servers) for exceptional sustained throughput. A Spring Boot process running for hours becomes progressively more optimised.

**GraalVM Native Image** (AOT compilation) inverts this trade-off: it compiles bytecode to a native binary ahead of time, eliminating warmup at the cost of some JIT optimisations. Popular in serverless/container contexts where startup time is critical.

---

# 2. Java's Type System

Java's type system is static and strong — all types are declared explicitly at compile time and never change. This is the single largest mental shift coming from Python or JavaScript.

| Language | Typing |
|----------|--------|
| Python | Dynamic — types resolved at runtime, variables can change type |
| JavaScript | Dynamic + weakly typed — `"5" + 1 = "51"` |
| C++ | Static + strong — types fixed at compile time |
| Java | Static + strong — types fixed at compile time, no implicit coercion |

## Primitives vs. Reference Types

Java has a fundamental split not found in quite the same form in other major languages:

```
Java Types
├── Primitive Types    ← raw values, stored directly in memory (stack)
└── Reference Types   ← objects, stored as pointers to heap memory
```

### The 8 Primitive Types

| Type | Size | Range | Example |
|------|------|-------|---------|
| `byte` | 8-bit | -128 to 127 | `byte b = 100;` |
| `short` | 16-bit | -32,768 to 32,767 | `short s = 1000;` |
| `int` | 32-bit | ~±2.1 billion | `int x = 42;` |
| `long` | 64-bit | ~±9.2 quintillion | `long l = 42L;` |
| `float` | 32-bit | ~±3.4 × 10³⁸ | `float f = 3.14f;` |
| `double` | 64-bit | ~±1.7 × 10³⁰⁸ | `double d = 3.14;` |
| `char` | 16-bit | Unicode 0–65,535 | `char c = 'A';` |
| `boolean` | JVM-dependent | `true` / `false` | `boolean flag = true;` |

Once declared, a variable's type is permanent:

```java
int x = 42;
x = "hello"; // ❌ Compiler error — type is fixed
x = 42.5;    // ❌ Compiler error — loss of precision
```

### Reference Types

Everything that is not a primitive is a reference type — all classes, arrays, and interfaces. A reference variable holds a memory address pointing to an object on the heap, not the object itself (analogous to a managed pointer in C++).

```java
String name    = "Professor";    // reference type
int[]  scores  = {95, 87, 91};  // array — also a reference type
```

Because reference types are pointers, they can point to nothing:

```java
String name = null;
System.out.println(name.length()); // 💥 NullPointerException at runtime
```

The **NullPointerException (NPE)** is Java's most infamous runtime error. Java 14+ produces detailed NPE messages identifying exactly which variable was null. The root cause is always the same: dereferencing a reference that holds `null`.

## Autoboxing — The Bridge Between Two Worlds

Java's Collections (`ArrayList`, `HashMap`, etc.) cannot store primitives — they require objects. Java provides **wrapper classes** as object equivalents, and automatically converts between them:

| Primitive | Wrapper |
|-----------|---------|
| `int` | `Integer` |
| `double` | `Double` |
| `boolean` | `Boolean` |
| `long` | `Long` |
| `char` | `Character` |

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(42);          // autoboxing:   int 42 → Integer(42)
int x = numbers.get(0);  // unboxing:      Integer(42) → int 42
```

> ⚠️ **Performance Trap:** Each autoboxing/unboxing operation allocates or deallocates a heap object. In a hot loop running millions of iterations per second, this generates significant GC pressure:
>
> ```java
> // Every iteration boxes int → Integer (1M heap allocations)
> List<Integer> list = new ArrayList<>();
> for (int i = 0; i < 1_000_000; i++) {
>     list.add(i);
> }
> ```
>
> **Fix:** Use primitive arrays (`int[]`) for performance-critical paths. For dynamic collections, consider libraries like **Eclipse Collections** which offer primitive-backed `IntList`, eliminating autoboxing entirely.

## Type Casting

Narrowing (precision loss) requires explicit cast:

```java
double d = 9.99;
int i = (int) d; // explicit — truncates to 9
```

Widening is implicit and always safe:

```java
int x = 42;
double d = x; // implicit widening
```

Unlike JavaScript, Java will never silently discard data — a narrowing assignment without a cast is a compile error.

---

# 3. Object-Oriented Design in Java

Java's OOP model is more opinionated than C++ or Python. The most significant architectural decision Java made is the **removal of multiple class inheritance**, replaced with a more powerful interface system. Understanding interfaces is the single most important thing for enterprise Java design.

## The Four Pillars — Java's Implementation

### 1. Encapsulation — Access Modifiers

Java enforces four access levels at compile time:

| Modifier | Same Class | Same Package | Subclass | Everywhere |
|----------|-----------|--------------|----------|------------|
| `private` | ✅ | ❌ | ❌ | ❌ |
| *(default)* | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

Standard enterprise pattern — private fields, public methods with validation:

```java
public class BankAccount {
    private double balance;
    private String accountNumber;

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount <= 0) throw new IllegalArgumentException("Amount must be positive");
        this.balance += amount; // business rule enforced — external code cannot bypass this
    }
}
```

### 2. Inheritance — `extends`

```java
public class Animal {
    protected String name;
    public Animal(String name) { this.name = name; }
    public String speak() { return "..."; }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name); // must call parent constructor explicitly
    }

    @Override
    public String speak() { return "Woof"; }
}
```

Key distinctions from C++:
- Java is **single inheritance only** for classes — no diamond problem
- `super` calls parent constructor or method — equivalent to `Base::method()` in C++
- `@Override` is **compiler-verified** — a typo in the method name becomes a compile error rather than silently creating a new method. This does not exist in C++.
- All Java classes implicitly extend `Object` — the root of the entire class hierarchy

### 3. Interfaces — Java's Most Powerful Construct

An interface is a **pure contract** — it defines what a class must do, with no prescription of how:

```java
public interface Payable {
    double calculatePay();       // abstract by default
    String getPaymentDetails();  // abstract by default
}

public class Employee implements Payable {
    private double hourlyRate;
    private int hoursWorked;

    @Override
    public double calculatePay() { return hourlyRate * hoursWorked; }

    @Override
    public String getPaymentDetails() { return "Employee: $" + calculatePay(); }
}
```

The critical advantage — a class can implement **multiple interfaces**:

```java
public class Contractor implements Payable, Taxable, Auditable {
    // must fulfil ALL three contracts
}
```

This is Java's answer to multiple inheritance — not inheriting implementation but inheriting contracts. It is the foundation of virtually every enterprise design pattern.

**Java 8+ Default Methods** allow interfaces to provide a fallback implementation without breaking all existing implementors — essential when evolving an interface used by hundreds of classes across a large codebase:

```java
public interface Loggable {
    void log(String message);

    default void logError(String message) {  // existing implementors inherit this for free
        log("[ERROR] " + message);
    }
}
```

### 4. Abstract Classes — The Middle Ground

An abstract class sits between a concrete class and an interface: it can have both concrete and abstract methods, can hold fields and constructors, but cannot be instantiated directly.

```java
public abstract class Shape {
    private String colour;                     // concrete field

    public Shape(String colour) { this.colour = colour; }  // concrete constructor

    public String getColour() { return colour; }           // concrete method

    public abstract double area();             // forces subclasses to implement
    public abstract double perimeter();
}

public class Circle extends Shape {
    private double radius;

    public Circle(String colour, double radius) { super(colour); this.radius = radius; }

    @Override public double area()      { return Math.PI * radius * radius; }
    @Override public double perimeter() { return 2 * Math.PI * radius; }
}
```

## Interface vs. Abstract Class — The Decision Framework

This distinction is a core enterprise Java interview question. The precise mental model:

| | Interface | Abstract Class |
|-|-----------|----------------|
| **Use when** | Defining a capability / contract | Defining a base type with shared implementation |
| **Multiple?** | ✅ A class implements many | ❌ A class extends only one |
| **Fields** | Only `static final` constants | Any fields |
| **Constructor** | ❌ | ✅ |
| **Real-world examples** | `Serializable`, `Comparable`, `Runnable` | `HttpServlet`, `AbstractList` |

**Heuristic:** Interface = "can do" relationship. Abstract Class = "is a" relationship with shared behaviour.

## Polymorphism in Practice

The real payoff — calling code depends only on the interface, completely unaware of the concrete type beneath:

```java
List<Payable> payroll = new ArrayList<>();
payroll.add(new Employee(...));
payroll.add(new Contractor(...));
payroll.add(new Intern(...));

double total = 0;
for (Payable p : payroll) {
    total += p.calculatePay(); // correct implementation dispatched at runtime
}
```

**Design example from the session** — a payment processing system with shared fraud detection logic:

```
<<interface>> Payment
    + process()
    + refund()
         ↑                    ↑
AbstractFraudPayment      CryptoPayment
  + detectFraud()           (concrete)
         ↑            ↑
  CreditCard      BankTransfer
```

`AbstractFraudPayment` holds shared fraud logic as a concrete method. `CryptoPayment` implements `Payment` directly without that overhead. External services depend only on `Payment` — maximum decoupling.

---

# 4. Memory Management & Garbage Collection

Coming from C++, where you own memory entirely (`new` / `delete`), Java's GC is a liberation — but not magic. It operates on precise rules that every backend engineer must understand. Misunderstanding the GC is one of the most common sources of production performance issues: memory leaks, GC pauses, and `OutOfMemoryError`.

## The JVM Memory Structure

```
JVM Memory
├── Heap                      ← Where all objects live
│   ├── Young Generation
│   │   ├── Eden Space
│   │   ├── Survivor 0 (S0)
│   │   └── Survivor 1 (S1)
│   └── Old Generation (Tenured)
├── Metaspace                 ← Class metadata (replaced PermGen in Java 8)
├── Stack                     ← Per-thread; stores stack frames & primitives
└── PC Registers / Native     ← JVM internals
```

### Stack vs. Heap

| | Stack | Heap |
|-|-------|------|
| **Stores** | Primitives, reference variables (not objects), method frames | All objects |
| **Managed by** | Auto — frame destroyed on method return | Garbage Collector |
| **Speed** | Extremely fast (pointer increment) | Slower — GC overhead |
| **Scope** | Per-thread | Shared across all threads |

```java
public void processPayment() {
    int amount = 500;           // stack — primitive value
    String currency = "USD";    // stack — reference variable only
    Payment payment = new Payment(amount); // stack holds reference;
                                           // Payment object lives on HEAP
}
// method returns — stack frame destroyed
// 'payment' reference gone → Payment object now eligible for GC
```

## GC Eligibility — Reachability, Not Reference Counting

The GC uses **reachability analysis**, not reference counting (unlike Python). It traces all live references from **GC Roots** (active threads, static variables, stack locals). Anything not reachable from a GC Root is eligible for collection.

```java
Payment p = new Payment(); // object reachable via 'p'
p = null;                  // reference severed → unreachable → eligible for GC
                           // but not immediately collected
```

## Generational Garbage Collection

The JVM exploits the **Weak Generational Hypothesis** — most objects die young. In a typical backend, the vast majority of objects (request DTOs, temp calculations) are created and discarded within milliseconds. Very few objects live long (connection pools, caches, config).

**Young Generation — Minor GC (frequent, cheap):**

```
New Object → Eden Space
                ↓ (Eden fills → Minor GC)
          [live] → S0
                      ↓ (next Minor GC)
                 [live] → S1 → S0 → S1 → ...
                                          ↓ (age ≥ 15)
                                    Old Generation
          [dead] → collected immediately
```

**Old Generation — Major/Full GC (infrequent, expensive):**

Full GCs are expensive — they can cause **stop-the-world pauses** where the entire application halts. A 500ms Full GC pause in a system processing thousands of requests per second is a production incident.

## GC Algorithms

| Collector | Default In | Strategy | Best For |
|-----------|-----------|----------|----------|
| Serial GC | — | Single-threaded | Small apps only |
| Parallel GC | Pre-Java 9 | Multi-threaded, stop-the-world | High-throughput batch |
| **G1GC** | **Java 9+** | Concurrent, region-based | **Enterprise backends (default)** |
| ZGC | Java 15+ | Concurrent, sub-ms pauses | Latency-sensitive systems |
| Shenandoah | Java 12+ | Concurrent compaction | Low-latency systems |

For most Spring Boot backends, G1GC is the right choice. For real-time pricing engines or trading platforms, ZGC offers pause times under 1ms regardless of heap size.

## Memory Leaks — Still Possible in Java

> ⚠️ **Common misconception:** "Java has a GC, so no memory leaks." This is false.
>
> A Java memory leak occurs when objects remain **reachable but no longer needed**. The GC cannot collect them because references still exist.

The most common cause in backend systems — static collections that grow unbounded:

```java
public class SessionManager {
    // static — lives for entire application lifetime
    private static Map<String, UserSession> activeSessions = new HashMap<>();

    public void createSession(String id, UserSession session) {
        activeSessions.put(id, session);
    }
    // developer forgot removeSession() → sessions accumulate forever → OutOfMemoryError
}
```

**Observable production symptoms before OOM:**
1. Full GC pauses becoming increasingly frequent as Old Generation fills
2. Response times degrading as JVM spends more time collecting than executing
3. Finally — `java.lang.OutOfMemoryError: Java heap space`

Other common leak sources: event listeners not deregistered, `ThreadLocal` variables not cleaned up, caches with no eviction policy.

**Fix — bounded collection with eviction:**

```java
// Option 1 — bounded deque, evict oldest
private static final int MAX_HISTORY = 1000;
private static Deque<Report> reportHistory = new ArrayDeque<>();

reportHistory.addLast(report);
if (reportHistory.size() > MAX_HISTORY) {
    reportHistory.removeFirst();
}

// Option 2 — never use static in-memory collections as persistent storage
reportRepository.save(report); // that's what databases are for
```

## GC-Hostile vs. GC-Friendly Patterns

```java
// ❌ GC-hostile — creates a new String object on every iteration
public String buildReport(List<Order> orders) {
    String result = "";
    for (Order order : orders) {
        result += order.toString(); // thousands of dead String objects → GC pressure
    }
    return result;
}

// ✅ GC-friendly — one object, mutated in place
public String buildReport(List<Order> orders) {
    StringBuilder sb = new StringBuilder();
    for (Order order : orders) {
        sb.append(order.toString());
    }
    return sb.toString(); // one final String allocation
}
```

---

# 5. Java Collections Framework

Choosing the right collection is a backend performance decision, not just a correctness one. The wrong choice can turn O(1) operations into O(n) under load.

## The Hierarchy

```
<<interface>> Iterable
        ↓
<<interface>> Collection
    ├── <<interface>> List
    ├── <<interface>> Set
    └── <<interface>> Queue

<<interface>> Map          ← separate hierarchy — does not extend Collection
```

> ⚠️ `Map` is not a `Collection`. It maps keys to values rather than storing individual elements — a common surprise for Python developers where `dict` is just another container.

## List — Ordered, Indexed, Allows Duplicates

### ArrayList vs. LinkedList

| Operation | ArrayList | LinkedList |
|-----------|-----------|------------|
| `get(index)` | **O(1)** | O(n) |
| `add(end)` | O(1) amortised | O(1) |
| `add(middle)` | O(n) | O(1) |
| `remove(middle)` | O(n) | O(1) |
| `contains()` | O(n) | O(n) |

```java
// ArrayList — default capacity 10; grows to 1.5x on overflow
List<String> names = new ArrayList<>();

// For known large sizes, initialise with capacity to avoid repeated resizing
List<Order> orders = new ArrayList<>(10_000);
```

In practice, `ArrayList` outperforms `LinkedList` in the **vast majority of backend use cases** — including most insertions — due to **CPU cache locality**. Array elements are contiguous in memory; `LinkedList` nodes are scattered across the heap, causing cache misses. Always benchmark before choosing `LinkedList`.

## Set — Unordered, No Duplicates

| Implementation | Ordering | Lookup | Use When |
|----------------|----------|--------|----------|
| `HashSet` | None | O(1) | Fast membership testing |
| `LinkedHashSet` | Insertion order | O(1) | Ordered + no duplicates |
| `TreeSet` | Sorted | O(log n) | Sorted unique elements |

```java
// HashSet — the critical advantage is O(1) lookup, not just "no duplicates"
Set<String> processedIds = new HashSet<>();
processedIds.contains(transactionId); // O(1) — checked millions of times efficiently
processedIds.add(transactionId);
```

> ⚠️ A `List` also prevents reprocessing but at O(n) — catastrophic at millions of membership checks. Always reach for `HashSet` when the question is "have I seen this before?"

## Map — Key-Value Storage

The most heavily used collection in enterprise backend development.

### HashMap

```java
Map<String, UserProfile> cache = new HashMap<>();
cache.put("u-001", profile);         // O(1)
cache.get("u-001");                  // O(1)
cache.containsKey("u-001");          // O(1)
cache.getOrDefault("u-999", null);   // O(1)
```

**Internal mechanics:**
- Keys are hashed via `hashCode()` into buckets
- Collisions stored as a linked list within a bucket
- Java 8+: buckets exceeding 8 entries convert to a balanced tree — preventing O(n) worst-case
- Default load factor `0.75` — when 75% full, resizes to 2x and rehashes

```java
// Pre-size for known large maps — avoids rehashing overhead
Map<String, Order> orders = new HashMap<>(10_000, 0.75f);
```

### LinkedHashMap — LRU Cache Pattern

```java
// Classic LRU cache — evicts oldest entry when over capacity
Map<String, Report> lruCache = new LinkedHashMap<>(100, 0.75f, true) {
    @Override
    protected boolean removeEldestEntry(Map.Entry eldest) {
        return size() > 100;
    }
};
```

### TreeMap — Sorted Keys / Range Queries

```java
TreeMap<LocalDate, List<Order>> ordersByDate = new TreeMap<>();
ordersByDate.firstKey();                    // earliest date — O(log n)
ordersByDate.lastKey();                     // latest date — O(log n)
ordersByDate.subMap(startDate, endDate);    // range query — very powerful
```

`TreeMap` is invaluable for time-series data, leaderboards, or any backend feature requiring range queries on keys.

> ⚠️ **Critical interview pitfall:** `TreeMap` sorts by **key**. When you need results sorted by **value** (e.g., userId → totalSpend, sorted by spend), accumulate with `HashMap` then sort the entry set at report time:
>
> ```java
> Map<String, Double> spendMap = new HashMap<>();
> spendMap.merge(userId, transaction.getAmount(), Double::sum);
>
> // sort by value descending at report time
> spendMap.entrySet().stream()
>     .sorted(Map.Entry.<String, Double>comparingByValue().reversed())
>     .forEach(e -> report.add(e.getKey(), e.getValue()));
> ```

## Queue & Deque — FIFO / Double-Ended Processing

```java
// ArrayDeque — the correct default for both queue and stack use cases
Deque<Transaction> queue = new ArrayDeque<>();
queue.addLast(transaction);          // enqueue — O(1)
queue.addFirst(priorityTransaction); // high-priority front insertion — O(1)
queue.pollFirst();                   // dequeue — O(1)
```

`ArrayDeque` is faster than `LinkedList` as a queue and faster than `Stack` as a stack. It should be your default for both use cases.

## Program to Interfaces

```java
// ✅ Correct — declare with the interface type
List<Order>       orders = new ArrayList<>();
Map<String, User> users  = new HashMap<>();
Set<String>       ids    = new HashSet<>();

// ❌ Avoid — locks you to the implementation
ArrayList<Order> orders = new ArrayList<>();
```

Swapping implementations (e.g., `ArrayList` → `LinkedList`, `HashMap` → `TreeMap`) then requires changing only one word. This is the Open/Closed Principle in practice.

## Collections Utility Class

`Collections` provides static utility methods that operate on any collection:

```java
List<Integer> numbers = new ArrayList<>(List.of(3, 1, 4, 1, 5, 9));

Collections.sort(numbers);               // sorts in place
Collections.reverse(numbers);            // reverses in place
Collections.shuffle(numbers);            // randomises order
Collections.max(numbers);               // finds maximum
int freq = Collections.frequency(numbers, 1); // counts occurrences

// Wraps any collection in a synchronised decorator — thread-safe reads/writes
List<Order> threadSafe = Collections.synchronizedList(new ArrayList<>());
```

> ⚠️ `Collections.synchronizedList()` synchronises individual operations but **not** compound operations (check-then-act). For full concurrency safety, prefer `CopyOnWriteArrayList` for read-heavy workloads or a `ConcurrentHashMap`-backed set — covered in the Concurrency trunk.

## Quick Selection Guide

| Need | Use |
|------|-----|
| Ordered list, frequent reads | `ArrayList` |
| Frequent front insertion/deletion | `ArrayDeque` |
| Fast membership testing | `HashSet` |
| Sorted unique elements | `TreeSet` |
| Key-value, fast lookup | `HashMap` |
| Key-value, insertion order | `LinkedHashMap` |
| Key-value, sorted keys / range queries | `TreeMap` |
| FIFO task queue | `ArrayDeque` |

---

# 6. Java I/O and NIO

Java's I/O architecture comes in two distinct generations. The choice between them underpins every high-throughput backend system and is the reason frameworks like Netty and Spring WebFlux exist.

## The Two I/O Models

| | `java.io` | `java.nio` |
|-|-----------|------------|
| **Introduced** | Java 1.0 | Java 1.4 |
| **Model** | Stream-based, blocking | Buffer-based, non-blocking capable |
| **Paradigm** | One thread per connection | Many connections per thread |

## Classic I/O — `java.io`

Built around unidirectional streams (`InputStream` / `OutputStream`). Every `read()` or `write()` call blocks the calling thread until data is available:

```
Thread calls read()
        ↓
Thread suspended — waiting for data
        ↑
OS signals data ready
        ↓
Thread resumes
```

Classic I/O uses a **decorator pattern** to layer capabilities — always use `BufferedInputStream` in production; without it, every `read()` hits the OS directly:

```java
try (BufferedReader reader = new BufferedReader(new FileReader("orders.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        processOrder(line);
    }
}
```

**The scaling problem:** A server handling 10,000 simultaneous connections requires 10,000 threads — each blocked waiting for data. Thread stacks alone consume gigabytes of memory doing nothing.

## NIO — `java.nio`

NIO replaces the stream model with three foundational concepts:

- **Buffers** — fixed-size memory blocks (replace direct byte passing)
- **Channels** — bidirectional data conduits (replace streams)
- **Selectors** — monitor many channels from one thread

### Buffers — Three State Variables

```java
ByteBuffer buffer = ByteBuffer.allocate(1024);
// state: position=0, limit=1024, capacity=1024

buffer.put("Hello Backend".getBytes()); // position advances to 13
buffer.flip();   // WRITE → READ: limit=13, position=0
buffer.get(data); // reads from position to limit
buffer.clear();  // reset: position=0, limit=1024
```

> ⚠️ **`flip()` is the most commonly forgotten NIO operation.** Writing to a buffer then reading without `flip()` reads garbage — `position` is at the end of your written data, not the beginning.

### Direct Buffers — Zero-Copy I/O

```java
ByteBuffer heapBuffer   = ByteBuffer.allocate(1024);       // JVM heap → OS buffer → destination (copy)
ByteBuffer directBuffer = ByteBuffer.allocateDirect(1024); // OS buffer → destination directly (zero-copy)
```

Direct buffers bypass the JVM heap — the OS transfers data directly to/from native memory. Higher allocation cost, but dramatically lower CPU overhead for high-throughput network I/O.

### `FileChannel.transferTo()` — Maximum File Transfer Efficiency

```java
// Zero-copy: data flows disk → kernel buffer → destination
// OS handles the transfer entirely — JVM not involved
source.transferTo(0, source.size(), destinationChannel);
```

Uses the OS `sendfile` syscall — the same mechanism used by Nginx. Orders of magnitude faster than reading into a JVM buffer and writing back out.

### Selectors — The Reactor Pattern

```java
Selector selector = Selector.open();
serverChannel.configureBlocking(false);
serverChannel.register(selector, SelectionKey.OP_ACCEPT);

while (true) {
    selector.select(); // blocks until any channel is ready
    for (SelectionKey key : selector.selectedKeys()) {
        if (key.isAcceptable()) {
            SocketChannel client = serverChannel.accept();
            client.configureBlocking(false);
            client.register(selector, SelectionKey.OP_READ);
        } else if (key.isReadable()) {
            // process data — only fires when data is actually ready
        }
    }
}
```

**Architectural comparison:**

```
Classic I/O — Thread Per Connection:         NIO — Selector Model:
Connection 1 → Thread 1 (blocked)           Connection 1 ─┐
Connection 2 → Thread 2 (blocked)           Connection 2 ─┤─→ Selector → 1 Thread
...                                          ...            │   (only active when data ready)
10,000 connections → 10,000 threads         Connection N ─┘
```

This is the **Reactor Pattern** — the architectural foundation of Netty, Spring WebFlux, and Node.js. Thread count stays constant regardless of connection count.

## NIO.2 — `Path` & `Files` API (Java 7+)

Prefer over `java.io.File` for all file operations:

```java
Path report = Path.of("orders", "report.txt");

Files.createDirectories(report.getParent());
Files.writeString(report, content);
String text       = Files.readString(report);
List<String> lines = Files.readAllLines(report);  // ⚠️ loads entire file — dangerous for large files
Files.copy(report, Path.of("backup/report.txt"));
Files.delete(report);
```

**For large files — always stream lazily:**

```java
try (Stream<String> lines = Files.lines(report)) {  // try-with-resources — holds a file handle
    lines.filter(line -> line.contains("ERROR"))
         .forEach(System.out::println);
} // file handle closed automatically
```

`Files.lines()` does not load the entire file into memory. `Files.readAllLines()` on a 10GB log file would exhaust your heap instantly.

**For truly massive files — stream directly to the caller without collecting:**

```java
// Most robust production pattern — caller controls consumption and closing
public Stream<String> extractErrors(String filePath) throws IOException {
    return Files.lines(Path.of(filePath))
                .filter(line -> line.contains("ERROR"));
    // caller is responsible for closing the stream with try-with-resources
}
```

Returning the stream avoids materialising any list in the method — even `Files.lines()` with `try-with-resources` inside the method would collect results before returning them. This pattern lets the caller decide how to consume (forEach, collect, limit) and when to close.

## Decision Framework

| Scenario | Use |
|----------|-----|
| Simple file read/write | NIO.2 `Files` API |
| Large file transfer | `FileChannel.transferTo()` |
| High-throughput network server | NIO Selector / Netty |
| Standard backend request handling | Classic I/O via framework abstraction |
| Reactive backend (WebFlux) | NIO under the hood — framework handles it |

In practice you rarely write raw NIO selector code in enterprise backends — frameworks abstract it. But understanding it is essential for diagnosing performance issues and making informed architecture decisions.

---

# 7. Networking in Java

Java's networking APIs operate at the **Transport Layer** — giving direct access to TCP and UDP while the OS handles everything below. HTTP is built on TCP; understanding sockets first makes HTTP behaviour predictable and debuggable.

## TCP vs. UDP

| Property | TCP | UDP |
|----------|-----|-----|
| Connection | Handshake required | Connectionless |
| Reliability | Guaranteed delivery, ordering, error checking | No guarantees |
| Speed | Slower | Faster |
| Use cases | HTTP, databases, file transfer | Video streaming, gaming, DNS |

Almost all enterprise backend communication — HTTP, JDBC, inter-service calls — is TCP-based.

## TCP Sockets — The Foundation

A socket is one endpoint of a two-way communication channel:

```java
// Server — binds to port, accepts connections
try (ServerSocket serverSocket = new ServerSocket(8080)) {
    while (true) {
        Socket clientSocket = serverSocket.accept(); // blocks until client connects
        new Thread(() -> handleClient(clientSocket)).start(); // ← thread-per-connection anti-pattern
    }
}

// Client — connects to server
try (Socket socket = new Socket("localhost", 8080);
     PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
     BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {
    out.println("ORDER-12345");
    String response = in.readLine(); // blocks until server responds
}
```

The `new Thread()` per connection is recognised immediately as the thread-per-connection anti-pattern. In production this is replaced with a thread pool executor or NIO selector.

### Production Socket Configuration

```java
Socket socket = new Socket();
socket.connect(new InetSocketAddress("payment-service", 8080), 3000); // connection timeout
socket.setSoTimeout(5000);   // read timeout — MANDATORY in production
socket.setKeepAlive(true);   // detect dead connections
socket.setTcpNoDelay(true);  // disable Nagle's algorithm — critical for low-latency inter-service calls
```

> ⚠️ `setSoTimeout()` is non-negotiable in production. Without it, a thread calling `read()` on a socket where the remote end has silently died will block **forever** — permanently consuming a thread pool slot. This is one of the most common causes of gradual thread pool exhaustion in backend services.

## Java HTTP Client (Java 11+)

A complete replacement for the aging `HttpURLConnection`. Create once and reuse — it manages an internal connection pool:

```java
HttpClient client = HttpClient.newBuilder()
    .connectTimeout(Duration.ofSeconds(3))
    .executor(executor)
    .followRedirects(HttpClient.Redirect.NORMAL)
    .build();
```

**Synchronous (blocking):**

```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://payment-service/api/charge"))
    .header("Authorization", "Bearer " + token)
    .POST(HttpRequest.BodyPublishers.ofString(jsonBody))
    .timeout(Duration.ofSeconds(5))  // request-level timeout — not setSoTimeout()
    .build();

HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
```

**Asynchronous (non-blocking) — returns `CompletableFuture`:**

```java
client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenApply(body -> parsePaymentResult(body))
      .exceptionally(ex -> PaymentResult.failed(ex.getMessage()))
      .thenAccept(result -> notifyOrderService(result));
```

**Parallel microservice calls:**

```java
CompletableFuture<String> inventoryFuture = client.sendAsync(inventoryReq, ofString()).thenApply(HttpResponse::body);
CompletableFuture<String> pricingFuture   = client.sendAsync(pricingReq,   ofString()).thenApply(HttpResponse::body);

// thenCombine — both results needed (allOf() discards them)
inventoryFuture.thenCombine(pricingFuture,
    (inventory, pricing) -> buildOrderSummary(inventory, pricing))
    .exceptionally(ex -> OrderSummary.failed());
```

> ⚠️ **`allOf()` vs `thenCombine()`:** `allOf()` returns `CompletableFuture<Void>` — the results are discarded. Use `thenCombine()` when you need both results to continue processing.

### Connection Pooling — One Client Instance

```java
// ✅ One instance — reuses pooled TCP connections
HttpClient client = HttpClient.newHttpClient(); // class-level field, injected

// ❌ New instance per call — new TCP handshake every time (~50-300ms per request)
HttpClient.newHttpClient().sendAsync(...);
```

Each new TCP + TLS handshake costs 50–300ms. `HttpClient` maintains a connection pool automatically — but only if you reuse the same instance. Always inject it as a dependency, not constructed inline. `static` avoids reinstantiation but couples the client permanently, making testing difficult. Constructor injection is the correct enterprise pattern.

## WebSockets — Persistent Bidirectional Communication

For real-time server-to-client communication — order status updates, live dashboards — HTTP request/response is insufficient. WebSocket maintains a persistent full-duplex connection; the server can push updates at any time without the client polling:

```java
HttpClient client = HttpClient.newHttpClient();
WebSocket webSocket = client.newWebSocketBuilder()
    .buildAsync(URI.create("wss://orders-service/live"),
        new WebSocket.Listener() {
            @Override
            public CompletionStage<?> onText(WebSocket ws, CharSequence data, boolean last) {
                System.out.println("Live update: " + data);
                return null;
            }
            @Override
            public void onError(WebSocket ws, Throwable error) {
                log.error("WebSocket error", error);
            }
            @Override
            public CompletionStage<?> onClose(WebSocket ws, int statusCode, String reason) {
                log.info("Connection closed: " + reason);
                return null;
            }
        })
    .join();

webSocket.sendText("SUBSCRIBE:ORDER-12345", true); // send message to server
```

Unlike HTTP, a WebSocket connection stays open — both sides can send messages freely. Essential for real-time backend features where push semantics are required.

---

# 8. JDBC & Database Connectivity

JDBC is Java's abstraction layer over database-specific drivers. Your code targets the standard `java.sql` interfaces regardless of which database sits underneath — switching from MySQL to PostgreSQL requires changing the driver dependency and connection URL, not your data access code.

```
Your Java Code
      ↓
JDBC API (java.sql)       ← standard interface you code against
      ↓
JDBC Driver               ← database-specific implementation
      ↓
Database (PostgreSQL, MySQL, Oracle...)
```

## Statement Types

### `Statement` — Never Use in Production

```java
// ❌ SQL injection vulnerability
String sql = "SELECT * FROM orders WHERE user_id = " + userId;
Statement stmt = conn.createStatement();
stmt.executeQuery(sql);
// userId = "' OR '1'='1" → returns every order in the database
```

### `PreparedStatement` — Always Use This

```java
// ✅ Parameters sent separately from SQL structure — injection structurally impossible
String sql = "SELECT * FROM orders WHERE user_id = ? AND status = ?";
try (PreparedStatement stmt = conn.prepareStatement(sql)) {
    stmt.setString(1, userId);
    stmt.setString(2, "ACTIVE");
    try (ResultSet rs = stmt.executeQuery()) {
        while (rs.next()) {
            orders.add(new Order(rs.getInt("order_id"), rs.getString("status")));
        }
    }
}
```

Two benefits: SQL injection prevention (structural, not sanitisation-based) and performance via statement plan caching — the database reuses the compiled execution plan on subsequent calls.

### `CallableStatement` — Stored Procedures

```java
try (CallableStatement stmt = connection.prepareCall("{call process_order(?)}")) {
    stmt.setInt(1, orderId);
    stmt.execute();
}
```

Use when the database encapsulates logic in stored procedures. Less common in modern Java backends where business logic lives in the application layer, but standard in legacy enterprise systems.

## Resource Management — try-with-resources is Mandatory

`Connection`, `PreparedStatement`, and `ResultSet` all hold external resources that must be explicitly closed:

```java
// ❌ Exception before close() leaves connection permanently open
Connection conn = DriverManager.getConnection(url, user, pass);
// exception thrown here — conn never closed

// ✅ try-with-resources — all resources closed regardless of exceptions
try (Connection conn     = dataSource.getConnection();
     PreparedStatement stmt = conn.prepareStatement(sql);
     ResultSet rs        = stmt.executeQuery()) {
    while (rs.next()) { processRow(rs); }
} // conn, stmt, rs all closed here
```

Unclosed `Connection` objects are one of the most common and damaging bugs in backend Java. Each leak holds a database session permanently — exhausting the database's connection limit and eventually taking down both the application and the database.

## Transactions

By default JDBC operates in auto-commit mode. For operations that must succeed or fail atomically:

```java
// Transfer $500 — both operations must commit or neither does
try (Connection conn = dataSource.getConnection()) {
    conn.setAutoCommit(false); // begin transaction
    try {
        debitStatement.executeUpdate();
        creditStatement.executeUpdate();
        conn.commit(); // both succeeded — commit atomically
    } catch (SQLException e) {
        conn.rollback(); // either failed — roll back entirely
        throw e;
    } finally {
        conn.setAutoCommit(true);
    }
}
```

Without a transaction, if debit succeeds and credit fails — money disappears. The transaction guarantees **atomicity** — the A in ACID.

## Connection Pooling — HikariCP

Opening a database connection involves a TCP handshake, authentication, and memory allocation — costing 50–200ms per connection. At 1,000 requests/second, creating a new connection per request is untenable.

A connection pool maintains pre-opened connections — borrow and return rather than create and destroy:

```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:postgresql://localhost:5432/orders_db");
config.setMaximumPoolSize(10);       // max connections to database
config.setMinimumIdle(5);            // min idle connections maintained
config.setConnectionTimeout(3000);   // max wait for connection from pool (ms)
config.setIdleTimeout(600_000);      // close idle connections after 10 min
config.setMaxLifetime(1_800_000);    // recycle connections after 30 min
config.setConnectionTestQuery("SELECT 1"); // verify connections are alive

HikariDataSource dataSource = new HikariDataSource(config);

// Usage — conn is returned to pool on close, not actually closed
try (Connection conn = dataSource.getConnection()) { ... }
```

**Pool sizing — the HikariCP formula:**

```
connections = (core_count × 2) + effective_spindle_count
```

For a 4-core server with SSD: `(4 × 2) + 1 = 9 ≈ 10 connections`. This seems counterintuitively small — but database operations are I/O-bound. More threads do not speed up I/O-bound work; they add context-switching overhead. Each PostgreSQL connection also consumes ~5–10MB on the database server — 100 connections = 500MB–1GB.

## Batch Operations — Bulk Inserts

```java
// ❌ 10,000 individual network round trips
for (Order order : orders) { stmt.executeUpdate(); }

// ✅ ~20 round trips — 50–100x throughput improvement
try (PreparedStatement stmt = conn.prepareStatement("INSERT INTO orders VALUES (?, ?, ?)")) {
    for (int i = 0; i < orders.size(); i++) {
        stmt.setInt(1, orders.get(i).getId());
        stmt.addBatch(); // queue locally
        if (i % 500 == 0) {
            stmt.executeBatch(); // send 500 rows at once
            stmt.clearBatch();
        }
    }
    stmt.executeBatch(); // final batch
}
```

## ResultSet Streaming — Large Query Results

```java
// ✅ Fetch 100 rows at a time — never holds all results in heap
PreparedStatement stmt = conn.prepareStatement(
    "SELECT * FROM orders WHERE year = 2024",
    ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY);
stmt.setFetchSize(100);
ResultSet rs = stmt.executeQuery();
while (rs.next()) { processOrder(rs); }
```

Fetching millions of rows into memory without `setFetchSize()` is the JDBC equivalent of `Files.readAllLines()` on a 10GB file.

---

# 9. Enterprise Design Patterns

Design patterns are named, reusable solutions to recurring design problems — blueprints for structuring class relationships, not code to copy. Four patterns appear in virtually every enterprise Java codebase.

## Pattern 1 — Singleton

**Problem:** Some resources must exist exactly once — a connection pool, configuration manager, logging service. Multiple instances waste resources or produce inconsistent state.

**Naive implementation — broken in multithreaded environments:**

```java
// ❌ Two threads can simultaneously see instance == null and both create an instance
public static ConfigurationManager getInstance() {
    if (instance == null) {
        instance = new ConfigurationManager(); // race condition
    }
    return instance;
}
```

**Thread-safe — double-checked locking:**

```java
private static volatile ConfigurationManager instance; // volatile — cross-thread visibility

public static ConfigurationManager getInstance() {
    if (instance == null) {                              // first check — no lock
        synchronized (ConfigurationManager.class) {
            if (instance == null) {                      // second check — with lock
                instance = new ConfigurationManager();
            }
        }
    }
    return instance;
}
```

The outer check avoids synchronisation overhead once the instance exists. The inner check prevents double instantiation if two threads pass the outer check simultaneously.

**Modern Java — Enum Singleton (preferred):**

```java
public enum ConfigurationManager {
    INSTANCE;
    private final Map<String, String> config = loadConfigFromFile();
    public String get(String key) { return config.getOrDefault(key, ""); }
}

String dbUrl = ConfigurationManager.INSTANCE.get("db.url");
```

Thread-safe by JVM specification — enum instances are initialised exactly once during class loading. Also handles serialisation correctly, which double-checked locking does not. This is Joshua Bloch's recommended approach (*Effective Java*).

> **In Spring Boot** — every Spring bean is Singleton-scoped by default. Spring manages the lifecycle for you; raw Singleton implementation is largely unnecessary in framework-managed code, but understanding it is essential for diagnosing bean scope issues.

## Pattern 2 — Factory

**Problem:** Your code needs to create objects but the exact type depends on runtime conditions. Scattering `new ConcreteClass()` throughout your codebase couples you rigidly to specific implementations.

```java
public interface PaymentProcessor { PaymentResult process(Payment payment); }
public class CreditCardProcessor implements PaymentProcessor { ... }
public class BankTransferProcessor implements PaymentProcessor { ... }
public class CryptoProcessor implements PaymentProcessor { ... }

// Factory — centralises creation logic
public class PaymentProcessorFactory {
    public static PaymentProcessor create(PaymentMethod method) {
        return switch (method) {
            case CREDIT_CARD    -> new CreditCardProcessor();
            case BANK_TRANSFER  -> new BankTransferProcessor();
            case CRYPTO         -> new CryptoProcessor();
            default -> throw new IllegalArgumentException("Unsupported: " + method);
        };
    }
}

// OrderService knows nothing about concrete implementations
PaymentProcessor processor = PaymentProcessorFactory.create(order.getPaymentMethod());
```

Adding a new payment method — `PayPalProcessor` — requires one new `case` in the factory. Zero changes to `OrderService`. This is the **Open/Closed Principle** — open for extension, closed for modification.

## Pattern 3 — Builder

**Problem:** Some objects require many parameters at construction. A constructor with 8+ parameters is unreadable, error-prone, and painful when most parameters are optional.

```java
// ❌ Telescoping constructor — impossible to read at the call site
Order order = new Order("ORD-1", "USR-1", 99.99, "GBP", "PENDING", null, LocalDateTime.now(), false);

// ✅ Builder — self-documenting, optional fields are obvious, object is immutable after build()
Order order = new Order.Builder("ORD-1", "USR-1", 99.99)
    .currency("USD")
    .status("CONFIRMED")
    .priority()
    .notes("Express delivery requested")
    .build(); // validates before constructing — throws if amount <= 0
```

The Builder's `build()` method is the validation gate — it enforces invariants before the immutable object is created. Every field in the resulting `Order` is `final` — the object is immutable and safe to share across threads without synchronisation.

**In enterprise Java:** `HttpRequest.newBuilder()` (Phase 7), `HikariConfig` — both are Builder pattern. Lombok's `@Builder` annotation generates the entire structure automatically.

## Pattern 4 — Strategy

**Problem:** A class needs to perform an operation, but the algorithm varies with context. Hardcoding variants with `if/else` produces brittle code that violates the Open/Closed Principle.

```java
// Strategy interface — defines the algorithm contract
public interface DiscountStrategy { double apply(double originalPrice); }

// Concrete strategies — each encapsulates one algorithm
public class PercentageDiscount implements DiscountStrategy {
    private final double percentage;
    public double apply(double price) { return price * (1 - percentage / 100); }
}

// Context class — uses whatever strategy is injected
public class PricingService {
    private DiscountStrategy discountStrategy;
    public PricingService(DiscountStrategy strategy) { this.discountStrategy = strategy; }
    public double calculatePrice(double basePrice) { return discountStrategy.apply(basePrice); }
}
```

Adding a new carrier requires a new `DiscountStrategy` implementation — `PricingService` never changes. `if/else` would require modifying `PricingService` every time, violating Open/Closed.

**Modern Java — lambdas replace simple concrete strategy classes entirely:**

```java
DiscountStrategy premiumDiscount = price -> price * 0.80;
DiscountStrategy bulkDiscount    = price -> price - 50.0;
PricingService service = new PricingService(premiumDiscount);
```

**Factory + Strategy — the common enterprise composition:**

```java
public class PricingServiceFactory {
    public static PricingService create(Customer customer, Order order) {
        if (customer.isPremium())        return new PricingService(price -> price * 0.80);
        else if (order.getQuantity() > 100) return new PricingService(price -> price - 50.0);
        else                             return new PricingService(price -> price);
    }
}
```

No pattern exists in isolation in production code — Factory selects the Strategy, Builder constructs the objects, Singleton provides the shared resources.

## Pattern Summary

| Pattern | Problem Solved | Key Benefit |
|---------|---------------|-------------|
| Singleton | One instance globally | Controlled shared resource access |
| Factory | Decouple creation from usage | Add new types without changing callers |
| Builder | Complex object construction | Readable, immutable, validated objects |
| Strategy | Algorithm varies at runtime | Swap behaviour without changing context |

---

# 10. Exception Handling & Logging

Robust exception handling and structured logging are the difference between a system you can diagnose in production and one that fails silently and mysteriously. They are also the first things senior engineers inspect during code review.

## Java's Exception Hierarchy

```
Throwable
├── Error                      ← JVM-level failures — NEVER catch these
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── Checked Exceptions     ← compiler enforced — must declare or catch
    │   ├── IOException
    │   └── SQLException
    └── RuntimeException       ← unchecked — no compiler enforcement
        ├── NullPointerException
        └── IllegalArgumentException
```

> ⚠️ **Never catch `Error`.** `OutOfMemoryError` and `StackOverflowError` represent unrecoverable JVM-level failures. Catching them creates a false impression of recovery while the JVM is in an undefined, corrupted state. Let the process terminate and let the orchestrator restart it.

## Checked vs. Unchecked Exceptions

**Checked** — compiler forces the caller to either catch or declare with `throws`. Use for recoverable conditions the caller can reasonably handle: file not found, network timeout, database unavailable.

**Unchecked** — propagate up the call stack freely. Use for programming errors — invalid arguments, violated preconditions, illegal state. The caller should have prevented these, not caught them.

**The enterprise position:** Modern enterprise Java — particularly Spring — strongly prefers unchecked exceptions. Checked exceptions force every intermediate layer to declare or wrap exceptions it cannot meaningfully handle, producing boilerplate `catch (Exception e) { throw new RuntimeException(e); }` throughout the codebase. Spring's own framework uses unchecked exceptions exclusively.

## Handling Strategies

**Strategy 1 — Catch and Recover:**
```java
public OrderResult processPayment(Payment payment) {
    try {
        return paymentGateway.charge(payment);
    } catch (PaymentGatewayException e) {
        log.warn("Primary gateway failed, attempting fallback", e);
        return fallbackGateway.charge(payment); // recover with fallback
    }
}
```

**Strategy 2 — Catch, Wrap, and Rethrow:** Translate low-level exceptions into domain-meaningful ones:
```java
public Order findOrder(String id) {
    try {
        return orderRepository.findById(id);
    } catch (SQLException e) {
        // caller should not know about SQL internals
        throw new OrderNotFoundException("Order not found: " + id, e); // ← always pass cause
    }
}
```

Always pass the original exception as the second argument. Losing the cause is one of the most common and costly logging mistakes in production — you lose the entire stack trace chain.

**Strategy 3 — Let It Propagate:** Not every layer should handle every exception. In Spring Boot, `@ControllerAdvice` / `@ExceptionHandler` provides a single centralised handler at the API boundary — exceptions propagate naturally upward and are translated into appropriate HTTP responses there.

## Custom Exception Design

```java
// Base domain exception — unchecked, carries domain context
public class OrderException extends RuntimeException {
    private final String orderId;
    private final ErrorCode errorCode;

    public OrderException(String message, String orderId, ErrorCode errorCode, Throwable cause) {
        super(message, cause); // always thread cause through
        this.orderId    = orderId;
        this.errorCode  = errorCode;
    }
}

// Specific exceptions extend base
public class OrderNotFoundException extends OrderException {
    public OrderNotFoundException(String orderId, Throwable cause) {
        super("Order not found: " + orderId, orderId, ErrorCode.NOT_FOUND, cause);
    }
}
```

Structured custom exceptions carry domain context — `orderId`, `errorCode` — that generic exceptions cannot. In production this context is invaluable for diagnosing failures without parsing string messages.

## SLF4J & Logback

```
Your Code → SLF4J API (facade) → Logback (implementation)
```

SLF4J is a logging facade — your code targets the interface, never a specific implementation. Swapping Logback for Log4j2 requires no application code changes. Same principle as JDBC over database drivers.

```java
private static final Logger log = LoggerFactory.getLogger(OrderService.class);
```

### Log Levels

| Level | Use For | Production Default |
|-------|---------|--------------------|
| TRACE | Extremely granular flow | Off |
| DEBUG | Diagnostic information | Off |
| INFO | Normal operational events | On |
| WARN | Recoverable issues, degraded state | On |
| ERROR | Failures, exceptions | On |

### Parameterised Logging — Mandatory

```java
// ❌ String concatenation — always evaluated, even if DEBUG is off
log.debug("Processing order: " + order.toString() + " for user: " + userId);

// ✅ Parameterised — evaluated ONLY if DEBUG level is enabled
log.debug("Processing order: {} for user: {}", order, userId);

// ❌ Loses full stack trace — only one line of exception info
log.error("Database error: " + e.getMessage());

// ✅ Full stack trace printed automatically by Logback
log.error("Database error", e);
```

At INFO level in production, every `debug()` call with string concatenation still builds the string and immediately discards it — wasting CPU on every hot path.

### Structured Logging — Production Standard

Plain text logs are difficult to query at scale. Use JSON output (via `LogstashEncoder` in Logback) to produce directly queryable log entries:

```json
{
  "timestamp": "2026-05-05T10:23:41.123Z",
  "level": "ERROR",
  "logger": "com.company.OrderService",
  "message": "Order processing failed",
  "orderId": "ORD-12345"
}
```

Structured logs are queryable in Elasticsearch, Datadog, Splunk — you can instantly find all errors for a specific `orderId` across millions of entries without fragile regex parsing.

### MDC — Correlation IDs

In a backend processing thousands of concurrent requests, logs from different requests are interleaved. MDC (Mapped Diagnostic Context) attaches request-scoped context to every log entry automatically:

```java
MDC.put("correlationId", UUID.randomUUID().toString());
MDC.put("userId", order.getUserId());
try {
    return orderService.process(order);
} finally {
    MDC.clear(); // critical — clear after request completes
}
```

Every subsequent log call in this thread automatically includes `correlationId` and `userId` — without passing them as parameters. In production, filter all logs for a single request across every service layer with one query.

---

# 11. Unit Testing with JUnit 5 & Mockito

Automated tests catch regressions at the moment they are introduced — before deployment. A unit test verifies one class in complete isolation from its dependencies.

```
▲
/ E2E  \          ← Full system, slow, few
/──────────\
/ Integration \   ← Multiple components, medium speed
/──────────────────\
/    Unit Tests     \ ← Isolated units, fast, numerous
──────────────────────
```

## JUnit 5 — Test Structure

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @BeforeEach void setUp()       { /* runs before EACH test */ }
    @AfterEach  void tearDown()    { /* runs after EACH test */  }
    @BeforeAll  static void init() { /* runs once before ALL tests */ }

    @Test
    @DisplayName("Order with null ID should throw IllegalArgumentException")
    void shouldRejectNullOrderId() {
        assertThrows(IllegalArgumentException.class, () ->
            new Order.Builder(null, "USR-1", 99.99).build()
        );
    }

    @Test
    @Disabled("Payment gateway not available in CI")
    void shouldProcessLivePayment() { }
}
```

### Core Assertions

```java
assertEquals(expected, actual);
assertEquals(99.99, total, 0.001);   // always use delta for floating point
assertTrue(order.isPriority());
assertNotNull(result);
assertThrows(OrderNotFoundException.class, () -> service.findOrder("invalid"));

// assertAll — all assertions evaluated even if one fails
assertAll(
    () -> assertEquals("ORD-1", order.getId()),
    () -> assertEquals("PENDING", order.getStatus()),
    () -> assertEquals(99.99, order.getAmount(), 0.001)
);
```

### Parameterised Tests

```java
// One test method — multiple input scenarios
@ParameterizedTest
@ValueSource(doubles = {-1.0, 0.0, -100.0})
void shouldRejectNonPositiveAmounts(double invalidAmount) {
    assertThrows(IllegalArgumentException.class, () ->
        new Order.Builder("ORD-1", "USR-1", invalidAmount).build()
    );
}

@ParameterizedTest
@CsvSource({"GBP, 100.0, 85.0", "EUR, 100.0, 92.0"})
void shouldConvertCurrencies(String currency, double amount, double expected) {
    assertEquals(expected, currencyService.convert(amount, currency, "USD"), 0.01);
}
```

## Mockito — Isolating Dependencies

`OrderService` depends on `OrderRepository` and `PaymentGateway`. In a unit test you never hit a real database or payment API — Mockito replaces them with controlled fakes:

```java
@ExtendWith(MockitoExtension.class)
class OrderServiceTest {

    @Mock OrderRepository orderRepository; // fake — no real database
    @Mock PaymentGateway  paymentGateway;  // fake — no real payment API
    @InjectMocks OrderService orderService; // real class — mocks injected automatically
}
```

### Stubbing — Define Mock Behaviour

```java
when(orderRepository.findById("ORD-1")).thenReturn(order);
when(paymentGateway.charge(order)).thenReturn(PaymentResult.success());

// Stub to throw
when(paymentGateway.charge(order)).thenThrow(new PaymentGatewayException("Insufficient funds"));
```

### Verification — Confirm Interactions

Beyond return values — confirm your code called its dependencies correctly:

```java
// Verify save() called exactly once with the right object
verify(orderRepository, times(1)).save(order);

// Verify payment was never attempted twice
verify(paymentGateway, times(1)).charge(any(Order.class));

// Critical — verify save is NEVER called after payment failure
verify(orderRepository, never()).save(any(Order.class));
```

`verify()` tests behaviour, not just return values. In a payment system, confirming that `save()` is **never** called after a failed payment is as important as confirming the exception is thrown.

### Argument Matchers

```java
verify(repository).save(any(Order.class));                      // any instance
verify(repository).save(argThat(o -> o.getStatus().equals("CONFIRMED"))); // field check
when(repository.findById(anyString())).thenReturn(order);       // stub with matcher
```

## What to Test

```
✅ Happy path        — normal successful execution
✅ Failure paths     — exceptions, invalid inputs, dependency failures
✅ Boundary conditions — empty collections, zero values, maximum values
✅ Business rules    — discount applied correctly, order rejected when out of stock
❌ Getters/setters, framework code, third-party libraries
```

A test suite with 70% meaningful coverage is far more valuable than 100% coverage achieved by testing trivial getters. **Test behaviour, not implementation.**

### Test Determinism

A test must produce the same result regardless of where or when it runs. Never rely on `LocalDateTime.now()` or system timezone defaults in tests — inject a fixed `Clock`:

```java
// Production code accepts a Clock
Order order = orderService.createOrder("ORD-1", clock);

// Test — always deterministic
Clock fixedClock = Clock.fixed(Instant.parse("2026-01-01T00:00:00Z"), ZoneOffset.UTC);
Order order = orderService.createOrder("ORD-1", fixedClock);
```

---

# 12. Performance Tuning & JVM Profiling

> **The golden rule: never optimise without measuring first.** Developers consistently guess wrong about where bottlenecks are. Profile first — optimise what the data tells you.

## Investigation Hierarchy

When a service degrades, investigate in this order:

1. **Application Code** — inefficient algorithms, N+1 queries *(80% of problems)*
2. **JVM / GC** — heap pressure, GC pauses
3. **Thread Contention** — lock contention, deadlocks, thread starvation
4. **Infrastructure** — CPU, memory, network, disk

Tuning GC flags on fundamentally inefficient code treats symptoms, not causes.

## Essential JVM Flags

```bash
java \
  -Xms2g                          # initial heap size
  -Xmx2g                          # max heap — same as initial in production
  -XX:+UseG1GC                    # G1 garbage collector
  -XX:MaxGCPauseMillis=200        # target max GC pause time
  -XX:+UseContainerSupport        # respect container CPU/memory limits
  -XX:MaxRAMPercentage=75.0       # use 75% of container memory for heap
  -XX:+HeapDumpOnOutOfMemoryError # capture heap dump on OOM — critical
  -XX:HeapDumpPath=/dumps/        # where to write heap dumps
  -XX:+ExitOnOutOfMemoryError     # crash cleanly on OOM — let orchestrator restart
  -jar application.jar
```

Setting `-Xms` equal to `-Xmx` prevents the JVM from dynamically resizing the heap — eliminating resize pauses and making memory usage predictable. The JVM always occupies the full allocation regardless of actual usage.

## Diagnosing GC Problems

| Symptom | Likely Cause |
|---------|-------------|
| High CPU, low throughput | Excessive GC — too many short-lived objects |
| Periodic latency spikes (~30 min cycle) | Full GC — Old Generation filling gradually |
| `OutOfMemoryError` | Memory leak |
| Heap increasing over time | Memory leak — confirmed |

A 30-minute slowdown-then-recovery cycle is the signature of a **Full GC cycle**: the Old Generation fills slowly, Full GC triggers a stop-the-world pause, heap is cleared, performance recovers — then repeats. Root cause: objects promoting to Old Generation faster than GC can reclaim. Start with a heap dump taken during the slowdown.

**G1GC Tuning — The Key Levers:**

```bash
-XX:MaxGCPauseMillis=200              # target max pause — not guaranteed, G1 best-efforts
-XX:G1HeapRegionSize=16m             # larger regions reduce fragmentation for large objects
-XX:InitiatingHeapOccupancyPercent=45 # start concurrent GC at 45% heap — prevent Full GC
```

Lowering `InitiatingHeapOccupancyPercent` starts the concurrent collection earlier, giving G1 more time to finish before the Old Generation fills completely — reducing the chance of a stop-the-world Full GC.

## Profiling Tools

**JVisualVM** — free, ships with JDK. CPU profiling, heap dump analysis, thread monitoring, deadlock detection. For development and local diagnosis.

**Java Flight Recorder (JFR)** — production-safe, less than 1% overhead. Continuously records GC, thread activity, I/O, and lock contention into a circular buffer. Essential for intermittent production issues:

```bash
java -XX:+FlightRecorder \
     -XX:StartFlightRecording=duration=60s,filename=recording.jfr \
     -jar application.jar
```

**`jstack` — Thread Dump Analysis:**

```bash
jstack <pid> > thread_dump.txt
```

A thread dump shows the exact state of every thread. Take **three dumps 10 seconds apart** — threads stuck in the same state across all three confirm a real blockage rather than transient activity:

```
"payment-thread-1" BLOCKED
  waiting to lock <PaymentGateway>
  locked <OrderRepository>

"payment-thread-2" BLOCKED
  waiting to lock <OrderRepository>
  locked <PaymentGateway>
```

This is a deadlock — circular wait. 50 threads all `BLOCKED` on the **same single lock** is a different problem: **lock contention** — one thread holds the lock for too long (e.g., during a slow DB call).

**Heap Dump Analysis:**

```bash
jmap -dump:format=b,file=heap.hprof <pid>
# Analyse with Eclipse Memory Analyser (MAT) or JVisualVM
```

Reveals: largest objects by retained size, object count by type (50M `String` objects → leak), and reference chains showing what's preventing GC.

## Application-Level Performance Patterns

**N+1 Query Problem — Most Common Backend Issue:**

```java
// ❌ 1000 orders = 1001 database round trips
List<Order> orders = orderRepository.findAll(); // 1 query
for (Order order : orders) {
    User user = userRepository.findById(order.getUserId()); // N queries
}

// ✅ 1 query with JOIN
List<Order> orders = orderRepository.findAllWithUsers();
```

Invisible in development with small datasets. Catastrophic in production. A service processing 100 orders in staging correctly can time out in production against 10,000 orders for exactly this reason.

**String Concatenation in Loops:**

```java
// ❌ O(n²) memory — new String object every iteration
String result = "";
for (Order order : orders) { result += order.toString(); }

// ✅ O(n) — one StringBuilder, one final allocation
StringBuilder sb = new StringBuilder();
for (Order order : orders) { sb.append(order.toString()); }
```

**Wrong Collection for Membership Testing:**

```java
// ❌ O(n) linear scan
List<String> processedIds = new ArrayList<>();
if (processedIds.contains(orderId)) { }

// ✅ O(1) hash lookup
Set<String> processedIds = new HashSet<>();
if (processedIds.contains(orderId)) { }
```

**JMH for Microbenchmarks:** Never use `System.currentTimeMillis()` to benchmark code — JIT compilation, GC, and OS scheduling introduce noise that makes naive timing meaningless. JMH handles JVM warmup, multiple iterations, and statistical analysis correctly:

```java
@BenchmarkMode(Mode.AverageTime)
@OutputTimeUnit(TimeUnit.MICROSECONDS)
@State(Scope.Benchmark)
public class StringBenchmark {
    private List<String> items = List.of("a", "b", "c" /* ... 1000 items */);

    @Benchmark
    public String concatenation() {
        String result = "";
        for (String item : items) result += item;
        return result;
    }

    @Benchmark
    public String stringBuilder() {
        StringBuilder sb = new StringBuilder();
        for (String item : items) sb.append(item);
        return sb.toString();
    }
}
```

`@State(Scope.Benchmark)` shares one instance across all benchmark threads. `@BenchmarkMode(Mode.AverageTime)` measures mean execution time. JMH runs multiple warmup iterations before recording results — letting the JIT fully optimise the code before measuring, which is how it will behave in production.

---

# 13. Dockerizing Java Applications

Docker solves the "works on my machine" problem at the infrastructure level. The same image runs identically on a developer laptop, CI server, and production.

## A Naive Dockerfile — And Its Problems

```dockerfile
FROM openjdk:21
COPY target/application.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Three production problems: the full JDK is ~600MB (production needs JRE only, ~180MB); there is no JVM memory tuning for containerised environments; and the entire JAR is one layer — every build invalidates the cache, making CI slow.

## Multi-Stage Build — Production Standard

```dockerfile
# ─── Stage 1: Build ───────────────────────────────────
FROM maven:3.9-eclipse-temurin-21 AS builder
WORKDIR /build

COPY pom.xml .
RUN mvn dependency:go-offline        # cache dependencies — layer invalidated only on pom.xml change

COPY src ./src
RUN mvn package -DskipTests

# ─── Stage 2: Runtime ─────────────────────────────────
FROM eclipse-temurin:21-jre-alpine   # JRE only — Alpine Linux — ~180MB
WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser                          # never run as root

COPY --from=builder /build/target/application.jar app.jar
EXPOSE 8080

ENTRYPOINT ["java",
  "-XX:+UseContainerSupport",         # respect container CPU/memory limits — CRITICAL
  "-XX:MaxRAMPercentage=75.0",        # use 75% of container memory for heap
  "-XX:+ExitOnOutOfMemoryError",      # crash cleanly on OOM — let orchestrator restart
  "-jar", "app.jar"]
```

> ⚠️ **`-XX:+UseContainerSupport` is critical.** Without it, the JVM reads the **host machine's** total RAM — not the container's memory limit. A container limited to 512MB on a 64GB host will configure a multi-gigabyte heap, immediately triggering the OS OOM killer. The crash appears as a killed process, not a Java `OutOfMemoryError`.

## Layer Caching — Optimising CI Build Speed

Copy `pom.xml` before source code so dependency downloading is a cached layer:

```dockerfile
COPY pom.xml .
RUN mvn dependency:go-offline  # ← cached unless pom.xml changes
COPY src ./src
RUN mvn package                # ← only this layer rebuilds on source change
```

| Build | Time |
|-------|------|
| Build 1 (cold) | Dependencies (2 min) + Compile (30s) = 2m 30s |
| Build 2+ | Cache hit + Compile (30s) = **30s** |

## Layered JAR — Spring Boot Optimisation

Spring Boot 2.3+ splits the JAR into layers ordered by change frequency:

```
Layer 1: dependencies          ← changes rarely
Layer 2: spring-boot-loader    ← changes rarely
Layer 3: snapshot-dependencies ← changes occasionally
Layer 4: application           ← changes every build (~500KB of your code)
```

Each layer is a distinct Docker layer. Only the `application` layer — your actual code — is rebuilt and pushed to the registry on each build. The ~150MB dependencies layer is cached permanently.

## Docker Compose — Local Development

```yaml
services:
  application:
    build: .
    ports: ["8080:8080"]
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/orders_db
    depends_on:
      postgres:
        condition: service_healthy  # wait for DB health check before starting app

  postgres:
    image: postgres:15-alpine
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app_user"]
      interval: 5s
      retries: 5
    volumes:
      - postgres_data:/var/lib/postgresql/data  # persist across restarts
```

## Configuration & Security

```dockerfile
# ❌ Never hardcode environment config — breaks the "one image, multiple environments" principle
ENV DB_URL=jdbc:postgresql://prod-db:5432/orders

# ✅ Pass at runtime — same image runs in dev, staging, production
docker run -e DB_URL=jdbc:postgresql://localhost:5432/orders_dev app
```

Credentials baked into a `Dockerfile` are baked into the image layer — visible in image history, permanent in Git, and requiring a full rebuild to rotate. Environment variables allow credential rotation with zero code changes and zero rebuilds.

**Security checklist:**
- Always run as a non-root user (`adduser` + `USER` directive)
- Pin base image versions — never use `latest` in production
- Scan images for vulnerabilities in CI (`docker scout`, `trivy`)

---

# 14. CI/CD Pipelines for Java

CI/CD makes deployment mechanical and repeatable — humans define the process once, machines execute it identically every time.

```
Developer pushes code
        ↓
CI Pipeline (automatic)
        ↓
Compile → Test → Analyse → Build Docker Image → Push to Registry
        ↓
CD Pipeline
        ↓
Deploy to Staging → Smoke Tests → [Manual Approval] → Deploy to Production
```

## GitHub Actions — Complete Java Pipeline

```yaml
# .github/workflows/ci.yml
name: Java CI Pipeline
on:
  push:        { branches: [main, develop] }
  pull_request: { branches: [main] }

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15-alpine
        env: { POSTGRES_DB: orders_test, POSTGRES_USER: test_user, POSTGRES_PASSWORD: test_pass }
        options: --health-cmd pg_isready --health-interval 10s --health-retries 5

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with: { java-version: '21', distribution: 'temurin' }

      # Cache Maven dependencies — same principle as Dockerfile layer caching
      - uses: actions/cache@v3
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}

      - run: mvn compile
      - run: mvn test
      - run: mvn verify -P integration-tests
        env: { DB_URL: jdbc:postgresql://localhost:5432/orders_test }
      - run: mvn jacoco:check          # fail if coverage < 80%
      - run: mvn checkstyle:check spotbugs:check

  build-docker-image:
    needs: build-and-test              # only runs if tests pass
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          password: ${{ secrets.GITHUB_TOKEN }}   # never hardcoded

      - uses: docker/build-push-action@v5
        with:
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.sha }}   # immutable tag
```

Tagging with `github.sha` — the Git commit hash — produces an immutable image tag. Every deployment is traceable to an exact commit. `latest` is mutable and makes rollbacks ambiguous.

## Quality Gates

**JaCoCo — Code Coverage:** Configure in `pom.xml` to fail the pipeline if line coverage drops below 80%.

**SpotBugs — Static Analysis:** Detects null dereferences, resource leaks, incorrect synchronisation, and SQL injection vulnerabilities at compile time — before the code runs.

## Deployment Strategies

**Blue-Green:** Two full environments. Switch load balancer from Blue (current) to Green (new). Old environment kept warm — rollback is a single switch, taking seconds. Zero downtime. Infrastructure cost doubles during deployment.

**Canary:** Route a small percentage of traffic (e.g., 10%) to the new version. Monitor error rates and latency. Gradually increase if healthy. Roll back if degraded. Limits blast radius — a bug affects 10% of users, not all. Standard at scale (Netflix, Google, Amazon).

**Rolling (Kubernetes default):** Replace pods incrementally while old version continues serving traffic. Rollout halts automatically if health checks fail on new pods.

## Rollback

```bash
kubectl rollout undo deployment/order-service           # instant rollback
kubectl rollout undo deployment/order-service --to-revision=3
kubectl rollout history deployment/order-service
```

Every deployment strategy must have a tested, rehearsed rollback procedure. Immutable `github.sha` image tags make rollback unambiguous — you always know exactly which image corresponds to which commit.

## Secrets Management

```yaml
# ❌ Never — credentials in pipeline code
env:
  DB_PASSWORD: "mysecretpassword"

# ✅ GitHub Secrets — encrypted, audited, never visible in logs
env:
  DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
```

A hardcoded URL or credential in a pipeline has two consequences: it is permanently in Git history (visible even after deletion) and it couples configuration to code — a database migration requires a code change, a new commit, and a new pipeline run rather than simply updating a secret.

In production, secrets are managed by HashiCorp Vault, AWS Secrets Manager, or Kubernetes Secrets — injected at runtime, rotated automatically, never stored in code.
