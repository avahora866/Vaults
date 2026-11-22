---
tags:
  - FORMAT
  - DataStructure
---
Binary search in trees applies to **Binary Search Trees (BSTs)**, not general binary trees. It leverages the property that each node’s left subtree contains smaller values and the right subtree contains larger ones.

A **BST** is a binary tree where:

* For any node `N`:

  * All values in `N.left` are **less than** `N.value`
  * All values in `N.right` are **greater than** `N.value`
* No duplicate values (in standard BSTs)

This ordering allows **binary search** to be performed in `O(log n)` time on average.

---

# Binary Search Algorithm on a BST

## Recursive Version (Pseudocode)

```java
Node search(Node root, int target) {
    if (root == null || root.value == target)
        return root;
    if (target < root.value)
        return search(root.left, target);
    else
        return search(root.right, target);
}
```

## Iterative Version (Pseudocode)

```java
Node search(Node root, int target) {
    while (root != null) {
        if (target == root.value)
            return root;
        else if (target < root.value)
            root = root.left;
        else
            root = root.right;
    }
    return null;
}
```

---

# Time and Space Complexity

| Case    | Time     | Space (Recursive) | Space (Iterative) |
| ------- | -------- | ----------------- | ----------------- |
| Best    | O(1)     | O(log n)          | O(1)              |
| Average | O(log n) | O(log n)          | O(1)              |
| Worst   | O(n)     | O(n)              | O(1)              |

Worst case happens when the tree is skewed (like a linked list). Balanced BSTs (e.g. AVL, Red-Black) avoid this.

---

# Example

Given BST:

```
        10
       /  \
      5    15
     / \     \
    3   7     20
```

To search for 7:

* Compare with 10 → go left
* Compare with 5 → go right
* Match 7 → return node

---

# When Binary Search Fails in Trees

Binary search only works **if** the tree satisfies the BST property. In a general binary tree (no ordering), you can’t eliminate half the tree, so you must do full DFS or BFS (`O(n)` time).

---

# Binary Search vs. Tree Traversal

| Operation           | Suitable Tree | Time Complexity |
| ------------------- | ------------- | --------------- |
| Binary Search       | BST           | O(log n) avg    |
| Traversal (DFS/BFS) | Any tree      | O(n)            |

---

# Applications

* Lookup in sets and maps (e.g., TreeSet, TreeMap)
* Auto-suggestion/autocomplete with ordered trees
* Range queries with augmented BSTs
* Memory-efficient search (vs. arrays with binary search)

---

# Common Variants and Enhancements

| Variant                      | Feature                               |
| ---------------------------- | ------------------------------------- |
| AVL Tree                     | Self-balancing BST                    |
| Red-Black Tree               | Balanced BST with color constraints   |
| Treap                        | BST + heap properties                 |
| Splay Tree                   | Recently accessed items moved to root |
| Segment Tree / Interval Tree | Augmented BSTs for range queries      |

---

# Summary

* Binary search in trees applies only to BSTs.
* It relies on the ordering property of BSTs.
* Time complexity is `O(log n)` on average if the tree is balanced.
* Always use iterative version when space is tight.
* Use balanced BSTs in practice to ensure good performance.