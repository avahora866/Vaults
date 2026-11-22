---
tags:
  - Java
---
Java provides a rich set of built-in data structures in the Java Collections Framework (JCF) and other utility classes. Here’s a categorized list of the key data structures available out of the box:

---

# Core Collection Interfaces and Implementations

These are part of `java.util` and `java.util.concurrent` packages.

## List (Ordered, Allows Duplicates)

 `ArrayList` (Resizable array-based list)  
 `LinkedList` (Doubly linked list)  
 `Vector` (Synchronized version of `ArrayList`)  
 `Stack` (LIFO stack, extends `Vector`)

## Set (No Duplicates)

 `HashSet` (Unordered, backed by `HashMap`)  
 `LinkedHashSet` (Maintains insertion order)  
 `TreeSet` (Sorted set, backed by `TreeMap`)  
 `EnumSet` (Optimized for `enums`)

## Queue & Deque (FIFO/LIFO Structures)

 `PriorityQueue` (Heap-based priority queue)  
 `ArrayDeque` (Resizable array-based deque)  
 `LinkedList` (Implements `Queue` and `Deque`)  
 `ConcurrentLinkedQueue` (Thread-safe non-blocking queue)  
 `LinkedBlockingQueue` (Blocking queue with optional capacity)  
 `PriorityBlockingQueue` (Thread-safe priority queue)  
 `ArrayBlockingQueue` (Fixed-size blocking queue)  
 `DelayQueue` (Holds elements until a delay expires)  
 `SynchronousQueue` (Single-element handoff queue)  
 `LinkedTransferQueue` (Optimized for [[Producer-Consumer|producer-consumer]])

## Map (Key-Value Pairs)

 `HashMap` (Unordered key-value storage)  
 `LinkedHashMap` (Maintains insertion order)  
 `TreeMap` (Sorted map, backed by `Red-Black Tree`)  
 `WeakHashMap` (Keys can be garbage collected)  
 `IdentityHashMap` (Uses reference equality instead of `.equals`)  
 `EnumMap` (Optimized for enum keys)  
 `ConcurrentHashMap` (Thread-safe high-performance map)  
 `ConcurrentSkipListMap` (Thread-safe sorted map)

---

# Specialized Data Structures

## BitSet

 `BitSet` (Memory-efficient storage for bits)

## Properties (Key-Value with String Keys and Values)

 `Properties` (Used for configuration files)

## Tuple-Like Structures

 `AbstractMap.SimpleEntry` (Key-value pair representation)  
 `AbstractMap.SimpleImmutableEntry` (Immutable key-value pair)

---

# Arrays & Utility Classes

 `Arrays` (Utility class for array operations)  
 `Collections` (Utility class for collections)  
 `Arrays.asList()` (Returns fixed-size list from an array)

---

# Thread-Safe Variants (java.util.concurrent)

 `CopyOnWriteArrayList` (Thread-safe `ArrayList`)  
 `CopyOnWriteArraySet` (Thread-safe `Set`)  
 `ConcurrentLinkedDeque` (Thread-safe deque)  
 `ConcurrentSkipListSet` (Thread-safe sorted set)

---

# Deque Implementations

 `ArrayDeque` (Resizable array-based deque)  
 `LinkedBlockingDeque` (Blocking deque for multi-threading)  
 `ConcurrentLinkedDeque` (Thread-safe non-blocking deque)

---
