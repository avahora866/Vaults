---
tags:
  - DataStructure
---

A **binary tree** is a hierarchical data structure where each node has at most **two children**: a **left child** and a **right child**.

* The top node is called the **root**.
* Nodes without children are called **leaves**.
* Each node contains data and references to its children.

---

# Types of Binary Trees

| Type                 | Description                                                        |
| -------------------- | ------------------------------------------------------------------ |
| Full Binary Tree     | Every node has 0 or 2 children                                     |
| Complete Binary Tree | All levels fully filled except possibly last, filled left to right |
| Perfect Binary Tree  | All internal nodes have two children and all leaves at same level  |
| Balanced Binary Tree | Height difference between subtrees is minimal                      |
| Binary Search Tree   | Left child < parent < right child for all nodes                    |

---

# Basic Operations

## Traversals

* **Inorder** (Left, Root, Right)
* **Preorder** (Root, Left, Right)
* **Postorder** (Left, Right, Root)
* **Level-order** (Breadth-first)

---

## Insertion

* For a **general binary tree**, insertion is often done at the first available position in **level order** (to maintain completeness).
* For **Binary Search Trees (BSTs)**, insertion follows the property:

  * If new key < node key → insert in left subtree
  * Else → insert in right subtree

### Insertion Algorithm (BST)

1. Start at root.
2. Compare key with current node:

   * If less, go left.
   * If greater, go right.
3. Repeat until you find a null child.
4. Insert new node there.

---

## Deletion

* Deletion in a **general binary tree** is complex and often involves replacing the node with the **deepest rightmost node**.
* In a **BST**, deletion has three cases:

  * **Node with no children (leaf):** Remove it directly.
  * **Node with one child:** Replace node with its child.
  * **Node with two children:**

    * Find the **inorder successor** (minimum node in right subtree) or **inorder predecessor** (maximum node in left subtree).
    * Replace the node’s value with successor/predecessor value.
    * Delete successor/predecessor node recursively.

---

## Example Java Code for BST Insert and Delete

```java
class Node {
    int key;
    Node left, right;
    Node(int key) {
        this.key = key;
        left = right = null;
    }
}

class BinarySearchTree {
    Node root;

    // Insert key
    Node insert(Node root, int key) {
        if (root == null) return new Node(key);
        if (key < root.key)
            root.left = insert(root.left, key);
        else if (key > root.key)
            root.right = insert(root.right, key);
        return root;
    }

    // Find minimum node in subtree
    Node minValueNode(Node root) {
        Node current = root;
        while (current.left != null)
            current = current.left;
        return current;
    }

    // Delete key
    Node deleteNode(Node root, int key) {
        if (root == null) return root;

        if (key < root.key)
            root.left = deleteNode(root.left, key);
        else if (key > root.key)
            root.right = deleteNode(root.right, key);
        else {
            // Node to be deleted found
            if (root.left == null)
                return root.right;
            else if (root.right == null)
                return root.left;

            // Node with two children
            Node temp = minValueNode(root.right);
            root.key = temp.key;
            root.right = deleteNode(root.right, temp.key);
        }
        return root;
    }
}
```

---

## Other Common Operations

* **Search**: Similar to insertion path in BST.
* **Find height**: Longest path from root to leaf.
* **Count nodes**: Total number of nodes.
* **Check if empty**: Root == null.

---

# Benefits of Binary Trees

* Simple hierarchical structure.
* Efficient searching and sorting in BST.
* Useful in expression parsing, priority queues, and binary heaps.

---

# Drawbacks

* Can become skewed, degrading to linked list in worst cases (unbalanced).
* Requires balancing for efficient operations (AVL, Red-Black Trees).

---

# Summary

| Operation | Description                              | Complexity (BST)       |
| --------- | ---------------------------------------- | ---------------------- |
| Insert    | Add new node following BST rules         | O(h) (h = tree height) |
| Delete    | Remove node with 0,1, or 2 children      | O(h)                   |
| Search    | Find node with specific key              | O(h)                   |
| Traverse  | Visit nodes in order (in/pre/post/level) | O(n)                   |

---