---
tags:
  - DataStructure
---
In **hash-based data structures** like `HashMap` and `ConcurrentHashMap`, a **table bucket** refers to a **slot in an internal array** where elements (key-value pairs) are stored.

In pre java 8 there was also a concept of segments which were considered as a submap of the map that grouped together buckets to allow concurrent access to multiple buckets at the same time. This was to get around the locking of the whole map for modification of one bucket