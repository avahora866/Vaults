---
tags:
  - DataStructure
---

A **Map** is an abstract data structure that stores data in **key-value pairs**. Each **key** is unique and maps to exactly one **value**.

* Keys are used to **retrieve**, **insert**, or **delete** values efficiently.
* Also known as **associative arrays** or **dictionaries** in many languages.

---

# Key Properties

* **Unique keys**: No duplicate keys allowed.
* **Values**: Can be duplicated or null.
* **Efficient lookup**: Access values by keys quickly.
* **Dynamic size**: Can grow or shrink as pairs are added or removed.

---

# Common Implementations

| Implementation  | Characteristics                      | Time Complexity (avg)             |
| --------------- | ------------------------------------ | --------------------------------- |
| Hash-based Map  | Uses hash table                      | O(1) insert, delete, lookup       |
| Tree-based Map  | Uses balanced tree (e.g., red-black) | O(log n) insert, delete, lookup   |
| Linked Hash Map | Hash table + linked list for order   | O(1) operations + maintains order |

---

# Operations and Complexity

| Operation        | Description                     | Typical Complexity           |
| ---------------- | ------------------------------- | ---------------------------- |
| Put(key, value)  | Insert or update key-value pair | O(1) (hash), O(log n) (tree) |
| Get(key)         | Retrieve value by key           | O(1) (hash), O(log n) (tree) |
| Remove(key)      | Delete key and its value        | O(1) (hash), O(log n) (tree) |
| ContainsKey(key) | Check if key exists             | O(1) (hash), O(log n) (tree) |
| Size()           | Number of key-value pairs       | O(1)                         |
| Iterate          | Traverse key-value pairs        | O(n)                         |

---

# Relationship to Dictionaries

* **Dictionaries** is a common name for Maps in many languages (e.g., Python, Swift).
* Both represent the same concept: a collection of **key-value pairs**.
* Terms like **Map**, **Dictionary**, **Associative Array**, and **Hash Table** are often used interchangeably but may differ slightly in implementation or language.

---

# Benefits of Using a Map

* **Fast lookup**: Access data by keys in constant or logarithmic time.
* **Flexible key-value storage**: Keys and values can be any type (depending on implementation).
* **Dynamic**: Size can change dynamically.
* **Easy updates**: Overwrite values by inserting with the same key.

---

# Disadvantages of Using a Map

* **Memory overhead**: Hash tables require extra space for hashing.
* **Unordered by default**: Unless using linked or tree maps.
* **Key constraints**: Keys must be unique and often immutable or hashable.
* **Collision handling**: Poor hash functions can degrade performance.

---

# Use Cases

* **Lookup tables** (e.g., phone books, symbol tables in compilers).
* **Caching** results for expensive computations.
* **Counting occurrences** (frequency maps).
* **Storing configuration** or settings with key-value pairs.
* **Associating metadata** with objects.
* **Implementing graphs** with adjacency maps.

---

# Summary

| Aspect                   | Description                             |
| ------------------------ | --------------------------------------- |
| Purpose                  | Store and access data by unique keys    |
| Key Feature              | Unique keys mapping to values           |
| Common Implementations   | Hash Map, Tree Map, Linked Hash Map     |
| Performance              | O(1) or O(log n) average for operations |
| Relation to Dictionaries | Same abstract concept, different names  |

---