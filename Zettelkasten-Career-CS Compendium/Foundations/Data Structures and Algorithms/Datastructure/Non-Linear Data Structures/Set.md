---
tags:
  - DataStructure
  - NOTES
---
A **Set** is an abstract data structure that stores **unique elements** without any particular order (unless specified by implementation). It supports efficient operations for:

* Adding elements
* Removing elements
* Checking membership

---

# Key Properties

* **Uniqueness**: No duplicate elements allowed.
* **Unordered**: Usually, elements are not stored in any guaranteed order.
* **Dynamic size**: Can grow or shrink as elements are added or removed.

---

# Common Implementations

| Implementation   | Characteristics                      | Time Complexity (avg)             |
| ---------------- | ------------------------------------ | --------------------------------- |
| Hash-based Set   | Uses hash table                      | O(1) insert, delete, lookup       |
| Tree-based Set   | Uses balanced tree (e.g., red-black) | O(log n) insert, delete, lookup   |
| Bit Set (Bitmap) | Uses bits to represent presence      | O(1) operations but limited scope |
| Linked Hash Set  | Hash + linked list for order         | O(1) operations + maintains order |

---

# Operations and Complexity

| Operation         | Description               | Typical Complexity           |
| ----------------- | ------------------------- | ---------------------------- |
| Add(element)      | Insert element if absent  | O(1) (hash), O(log n) (tree) |
| Remove(element)   | Delete element if present | O(1) (hash), O(log n) (tree) |
| Contains(element) | Check if element exists   | O(1) (hash), O(log n) (tree) |
| Size()            | Number of elements        | O(1)                         |
| Iterate           | Traverse all elements     | O(n)                         |

---

# Benefits of Using a Set

* **Ensures uniqueness**: Automatically prevents duplicates.
* **Efficient membership tests**: Quick checks if an element exists.
* **Simplifies logic**: Eliminates need for manual duplicate checks.
* **Useful in mathematical operations**: Union, intersection, difference.
* **Flexible implementations**: Can optimize for speed or memory.

---

# Disadvantages of Using a Set

* **No duplicate storage**: Can't represent multiple occurrences.
* **Unordered**: May not preserve insertion order unless specifically implemented.
* **Memory overhead**: Hash-based sets may require extra memory for hashing and storage.
* **Performance depends on implementation**: Poor hash functions or unbalanced trees degrade performance.

---

# Use Cases

* **Removing duplicates** from a list or array.
* **Membership checks** in large datasets (e.g., checking banned users).
* **Mathematical set operations** like union, intersection, and difference.
* **Tracking visited nodes** in graph traversal algorithms.
* **Caching unique items** without order requirements.
* **Implementing lookup tables** with quick access.

---

# Summary

| Aspect                 | Description                                  |
| ---------------------- | -------------------------------------------- |
| Purpose                | Store unique elements efficiently            |
| Key Feature            | No duplicates                                |
| Common Implementations | Hash set, Tree set, Bit set                  |
| Performance            | O(1) or O(log n) average for core operations |
| Limitations            | No duplicates, unordered by default          |

---