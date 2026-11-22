---
tags:
  - FLASHCARD
  - NOTES
  - DataStructure
---

An **N-ary tree** (or **generic tree**) is a tree data structure where each node can have **zero or more children**, with no fixed limit other than N (which can be any integer ≥ 2). Unlike binary trees, nodes are not restricted to two children.

* N-ary trees generalize binary trees.
* Nodes store data and have a list (or array) of child nodes.

---

# Structure

Each node typically contains:

* **Data** or value.
* A **list (or array)** of child nodes (0 to N children).

```
Node {
    value
    list of children
}
```

---

# Differences from Binary Trees

| Aspect            | Binary Tree             | N-ary Tree (Generic Tree)                   |
| ----------------- | ----------------------- | ------------------------------------------- |
| Children per node | At most 2               | Up to N (variable number)                   |
| Child storage     | Left and Right pointers | List/array of children                      |
| Use cases         | Expression trees, BSTs  | File systems, HTML DOM, organization charts |

---

# Common Operations

## Traversal

* **Preorder**: Visit node → visit children (recursively).
* **Postorder**: Visit children (recursively) → visit node.
* **Level-order**: Visit nodes level by level.

---

## Insertion

* Insert by adding a new child to a specified parent node.
* Usually involves locating the parent node and appending the new node to its children list.

---

## Deletion

* Delete a node by removing it from its parent's child list.
* All descendants of the node are also removed.
* May require locating the parent first.

---

# Advantages

* **Flexible structure**: Can model complex hierarchies naturally.
* **No fixed child limit**: Can represent trees where nodes have varying numbers of children.
* **Simple to implement** using lists for children.

---

# Disadvantages

* **Traversal complexity**: Variable number of children complicates some algorithms.
* **Memory usage**: Storing lists/arrays for children can increase memory overhead.
* **Less specialized**: No built-in ordering or balancing (unlike binary search trees).

---

# Use Cases

* **File systems**: Directories can have many files/folders.
* **HTML/XML DOM trees**: Elements with varying numbers of child elements.
* **Organization charts**: Employees with multiple subordinates.
* **Game trees**: Moves with multiple options.

---

# Example Java Implementation (Simplified)

```java
import java.util.ArrayList;
import java.util.List;

class NaryTreeNode<T> {
    T data;
    List<NaryTreeNode<T>> children;

    NaryTreeNode(T data) {
        this.data = data;
        children = new ArrayList<>();
    }

    void addChild(NaryTreeNode<T> child) {
        children.add(child);
    }

    void removeChild(NaryTreeNode<T> child) {
        children.remove(child);
    }
}

class NaryTree<T> {
    NaryTreeNode<T> root;

    NaryTree(T rootData) {
        root = new NaryTreeNode<>(rootData);
    }

    // Preorder traversal example
    void preorderTraversal(NaryTreeNode<T> node) {
        if (node == null) return;
        System.out.println(node.data);
        for (NaryTreeNode<T> child : node.children) {
            preorderTraversal(child);
        }
    }
}
```

---

# Summary

| Aspect           | Description                                     |
| ---------------- | ----------------------------------------------- |
| Definition       | Tree where nodes have up to N children          |
| Children storage | List or array of child nodes                    |
| Common ops       | Traversal (pre/post/level), insertion, deletion |
| Benefits         | Flexible, models varied hierarchies             |
| Limitations      | Traversal complexity, memory usage              |
| Use cases        | File systems, DOM trees, org charts, game trees |

---