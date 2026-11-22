---
tags:
  - FORMAT
  - Java
---
# Data Structures Built Primarily on Arrays

* **Array** (primitive built-in, contiguous memory)  
* **ArrayList** (backed by a resizable array)  
* **Vector** (like ArrayList but synchronized, backed by an array)  
* **Stack** (usually backed by Vector or ArrayList internally)  
* **String** (internally uses a `char[]` array to store characters)  
* **HashMap** / **HashSet** (the main storage is an array of buckets; buckets store entries as linked nodes or trees)  
* **PriorityQueue** (usually backed by a binary heap stored in an array)  
* **BitSet** (internally backed by a long array representing bits)  
* **Arrays utility methods** (like sorting, searching, obviously work on arrays)

---

# Data Structures Built Primarily on Linked Nodes (Objects with References)

* **LinkedList** (each element is a node with references to next/previous)  
* **Tree structures:**  
  * **Binary tree** (nodes with left and right child references)  
  * **Binary Search Tree (BST)**  
  * **AVL tree**  
  * **Red-Black tree** (used inside Java’s TreeMap, TreeSet)  
* **Graph nodes** (custom objects with lists of references to neighbor nodes)  
* **HashMap / HashSet buckets** (collision chains stored as linked lists or balanced trees with nodes holding references)  
* **Skip List** (nodes with multiple forward pointers, implemented as objects with references)  
* **Doubly linked list** (nodes with both next and previous references)

---

# Other Low-Level Structures or Hybrid Approaches

* **Arrays of linked nodes** — Hybrid approach in:  
  * **HashMap / HashSet** (array of buckets \+ linked list or tree nodes in buckets)  
* **Trees stored in arrays** (used in some implementations):  
  * **Binary heaps** in priority queues (array-based binary tree)  
  * Some specialized tree structures can be implemented in arrays for performance  
* **Primitive buffers and views:**  
  * **ByteBuffer**, **CharBuffer** in `java.nio` (wrap around arrays or native memory)  
* **Immutable collections** (e.g., those in `java.util.Collections` or third-party libs):  
  * May use arrays internally but expose different APIs and immutability guarantees  
* **Custom structures backed by hash tables**  
  * Some specialized maps or caches may combine arrays with linked nodes or trees

Some of these are self explanatory like linked list and graph nodes, however i will go into detail on some of the others that i believe will be helpful