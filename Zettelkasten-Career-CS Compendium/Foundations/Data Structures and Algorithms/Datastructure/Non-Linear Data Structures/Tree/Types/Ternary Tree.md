---
tags:
  - FLASHCARD
  - NOTES
  - DataStructure
---
A **ternary tree** is a special type of tree where each node has at most **three children**:

* **Left child**
* **Middle child**
* **Right child**

This is a specific case of an N-ary tree with N = 3.

---

# Structure

Each node contains:

* A data value.
* Three references/pointers to its children: left, middle, and right.

```
Node {
    value
    left child
    middle child
    right child
}
```

---

# Differences from Binary Trees

| Aspect            | Binary Tree                 | Ternary Tree                                    |
| ----------------- | --------------------------- | ----------------------------------------------- |
| Children per node | At most 2                   | At most 3                                       |
| Child references  | left, right                 | left, middle, right                             |
| Usage             | General purpose trees, BSTs | More specialized or problem-specific structures |

---

# Common Operations

## Traversal

* **Preorder**: Visit node → left → middle → right
* **Inorder (generalized)**: Left → node → middle → right (varies by use case)
* **Postorder**: Left → middle → right → node
* **Level-order**: Visit nodes level by level

---

## Insertion

* Depends on application logic.
* Typically, insert at the first available child slot (left, then middle, then right).
* In specialized trees like **ternary search trees**, insertion follows a comparison-based rule (similar to BST but with three children).

---

## Deletion

* Remove a node by replacing or reorganizing its children.
* In a generic ternary tree, deletion often involves removing the node and adjusting the parent's child references.
* In specialized ternary trees, deletion follows rules similar to BST deletion but adapted for three children.

---

# Use Cases

* **Ternary Search Trees**: A type of trie used for storing strings, where each node has up to three children representing characters less than, equal to, or greater than the current character.
* **Decision trees** with three branches at each decision point.
* Applications requiring three-way branching logic.

---

# Example Java Node Structure

```java
class TernaryTreeNode<T> {
    T data;
    TernaryTreeNode<T> left;
    TernaryTreeNode<T> middle;
    TernaryTreeNode<T> right;

    TernaryTreeNode(T data) {
        this.data = data;
        left = middle = right = null;
    }
}
```

---

# Summary

| Aspect      | Description                                                             |
| ----------- | ----------------------------------------------------------------------- |
| Definition  | Tree where each node has up to 3 children                               |
| Child nodes | Left, Middle, Right                                                     |
| Traversal   | Preorder, Postorder, Level-order                                        |
| Insertion   | Insert at available child slot or by comparison in ternary search trees |
| Use cases   | Ternary search trees, decision trees                                    |

---