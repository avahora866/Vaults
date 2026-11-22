---
tags:
  - DataStructure
---
Concurrent Hash Maps provide a structure that is:

* Thread safe  
* High throughput under high concurrency 

It is a replacement for the Collections.synchronisedMap as well as HashTable

# ConcurrentMap

`ConcurrentMap` is an extension of the Map interface. `ConcurrentMap` gives guidelines for valid implementations to provide thread-safety and memory-consistent [[Atomicity|atomic operations]].

# ConcurrentHashMap

ConcurrentHashMap is the main implementation of ConcurrentMap. It uses an array of nodes as table buckets instead of Linked Lists like in the HashMap.  
ConcurrentHashMaps use CAS operations for:

* putIfAbsent  
* Replace

But not in every operation.

## Bucket/Segments (Independent Locking)

In a normal HashMap when a thread is modifying it, then no other thread can modify it at the same time, usually this is achieved through locking the whole map. 

Segments are pre Java 8 concepts. They grouped buckets into a "sub-map" and were used to allow concurrent modification on multiple buckets at the same time. 

Segments were removed in Java 8 and were replaced with independent locking of buckets which provides more fine grained concurrency control

For cases when ordering of keys is required, we can use ConcurrentSkipListMap, a concurrent version of TreeMap

As a supplement for ConcurrentMap, ConcurrentNavigableMap supports total ordering of its keys (in ascending order by default) and is concurrently navigable