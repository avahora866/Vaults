---
tags:
  - DataStructure
---
A Red Black Tree (RBT) is a type of Binary Search Tree (BST) that is self-balancing. It maintains properties to help make sure it remains balanced during insertions and deletions. This makes operations like searching, insertion and deletion have a time complexity of O(log n)

# Properties of a Red-Black Tree:

* Node Color: Each node in the tree is either red or black.  
* Root Property: The root node is always black.  
* Leaf Property: All leaf nodes (null or NIL nodes) are black.  
* Red Node Property: No two red nodes can appear consecutively (i.e., a red node cannot have a red child).  
* Black Height Property: Every path from a node to its descendant NIL nodes must have the same number of black nodes. This number is referred to as the black height of the tree.  
* Balanced Depth: The tree ensures that the path from the root to any leaf is balanced, meaning that no path is more than twice as long as any other path.

# Insert

When a new node is inserted it is initially coloured red

# Skewness

Red-Black trees prevent skewness by making sure that after any insertion or deletion the tree performs any fix-up operations (rotations and recolouring) to restore the properties

A tree that is skewed (one side of the tree has more nodes than the other side) lowers performance. A balanced tree would usually have a O (log n) (time increases slowly as the tree grows) whilst a skewed tree would be O (n) (time increases proportional to the number of elements).

A heavily skewed tree can be thought of as basically a linked list

# Rotations

To maintain balance during insertion and deletion, the tree uses rotations:

* Left Rotation: A right child becomes the parent of its parent node.  
* Right Rotation: A left child becomes the parent of its parent node.

# Recolouring

When the colour of node is switched

# Example

```
// Insertion order 10 -> 5 -> 15 -> 3 -> 9 -> 20 -> 4
// TODO
```

