---
tags:
  - DataStructure
---
Below will be a list of datastructres and what they are best used for. These data structures are the main ones that programmers use NOT all the data structures. On Wikipedia there is a list of all the datastructures however I do not need to know them all but it is interesting.  
https://en.wikipedia.org/wiki/List_of_data_structures

# Linear Data structures

A linear data structure is a type of data structure that stores the data linearly or sequentially. In the linear data structure, data is arranged in such a way that one element is adjacent to its previous and the next element  
[List of data structures](joplin://#stacks)

- [[Array]]
- [[Linked Lists]]
- [[Data Structures and Algorithms/Datastructure/Linear Data Structures/Stack]]
- [[Queue]]

# Non-Linear Data structures

In a non-linear data structure, data is connected to its previous, next, and more elements like a complex structure. In simple terms, data is not organized sequentially in such a type of data structure.

- [[Map]]
- [[Set]]
- [[Tree]]
- [[Graph]]

# Arrays

An array is a linear data structure that collects elements of the same data type and stores them in  
[contiguous](../../../Software%20Development/Data%20Structures%20&%20Algorithms/Definitions.md) and adjacent memory locations. [[Array|Arrays]] work on an index system starting from 0 to (n-1), where n is the size of the array.

## Types of arrays

1.  [[Array|One-Dimensional Array (1-D)]]
2.  [[Array|Two-Dimensional Array (2-D)]]
3.  [[Array|Three-Dimensional Arrays (3-D)]]

# Linked Lists

A [[Linked Lists|linked list]] is a linear data structure that consists of a series of nodes connected by pointers. Each node contains data and a reference to the next node in the list. Unlike arrays, linked lists allow for efficient insertion or removal of elements from any position in the list, as the nodes are not stored [contiguously](../../../Software%20Development/Data%20Structures%20&%20Algorithms/Definitions.md) in memory.

## Types of Linked Lists

1.  [[Singly Linked List|Singly Linked Lists]]: A singly linked list is a linear data structure in which the elements are not stored in contiguous memory locations and each element is connected only to its next element using a pointer.
2.  [[Doubly Linked List|Doubly Linked Lists]]: A doubly linked list (DLL) is a special type of linked list in which each node contains a pointer to the previous node as well as the next node of the linked list.
3.  [[Circular Linked List|Circular Linked Lists]]: The circular linked list is a linked list where all nodes are connected to form a circle. In a circular linked list, the first node and the last node are connected to each other which forms a circle. There is no NULL at the end.
4.  Circular Doubly Linked Lists: A circular doubly linked list is defined as a circular linked list in which each node has two links connecting it to the previous node and the next node.
5.  Header Linked Lists: A header node is a special node that is found at the beginning of the list. A list that contains this type of node, is called the header-linked list. This type of list is useful when information other than that found in each node is needed.

# Stacks

A [[Data Structures and Algorithms/Datastructure/Linear Data Structures/Stack|stack]] is a data structure that follows a last in, first out (LIFO) protocol. The latest node added to a [[Data Structures and Algorithms/Datastructure/Linear Data Structures/Stack|stack]] is the node which is eligible to be removed first. If three nodes (a, b and, c) are added to a [[Data Structures and Algorithms/Datastructure/Linear Data Structures/Stack|stack]] in this exact same order, the node c must be removed first. The only way to remove or return the value of the node a is by removing the nodes c and b.

# Queues

A [[Queue|Queue]] Data Structure is a fundamental concept in computer science used for storing and managing data in a specific order. It follows the principle of “First in, First out” (FIFO), where the first element added to the queue is the first one to be removed. Queues are commonly used in various algorithms and applications for their simplicity and efficiency in managing data flow.

# Map / Tables

[[Map|Maps]] is also known as dictionaries or associative arrays, are fundamental data structures that allow you to efficiently store and retrieve data based on unique keys.  
Map data structures are important because they allow for efficient storage and retrieval of key-value pairs. [[Map|Maps]] provide the following benefits:

- Fast Lookup: Unordered [[Map|maps]] allow for constant-time (O(1)) average-case lookup of elements based on their unique keys.
- Efficient Insertion and Deletion: [[Map|Maps]] support fast insertion and deletion of key-value pairs, typically with logarithmic (O(log n)) or constant-time (O(1)) average-case complexity.
- Unique Keys: [[Map|Maps]] ensure that each key is unique, allowing for efficient association of data with specific identifiers.
- Flexible Data Storage: [[Map|Maps]] can store a wide variety of data types as both keys and values, providing a flexible and versatile data storage solution.
- Intuitive Representation: The key-value pair structure of [[Map|maps]] offers an intuitive way to model and represent real-world data relationships.

## Ordered Map

An ordered [[Map|map]] maintains the order in which key-value pairs are inserted. This means that iterating over the map will return the pairs in the order they were added.

## Unordered Map

An unordered [[Map|map]] does not maintain the order of key-value pairs. The order in which elements are returned during iteration is not guaranteed and may vary across different implementations or executions.

# Set

[[Set|Set]] Data Structure is a type of data structure which stores a collection of distinct elements.  
A [[Set|set]] can be implemented in various ways but the most common ways are:

- Hash-Based Set: the set is represented as a hash table where each element in the set is stored in a bucket based on its hash code.
- Tree-based set: In this implementation, the set is represented as a binary search tree where each node in the tree represents an element in the set.

## Ordered Sets

An Ordered [[Set|set]] is the common set data structure we are familiar with. It is generally implemented using balanced BSTs and it supports O(log n) lookups, insertions and deletion operations.

## Unordered Sets

An unordered [[Set|set]] is an unordered associative container implemented using a hash table where keys are hashed into indices of a hash table so that the insertion is always randomized. All operations on the unordered [[Set|set]] take constant time O(1) on an average which can go up to linear time O(n) in the worst case which depends on the internally used hash function, but practically they perform very well and generally provide a constant time lookup operation.

# Trees

[[Tree|Tree]] data structure is a specialized data structure to store data in hierarchical manner. It is used to organize and store data in the computer to be used more effectively. It consists of a central node, structural nodes, and sub-nodes, which are connected via edges. We can also say that tree data structure has roots, branches, and leaves connected.

## Properties of Tree Data Structure:

- Number of edges: An edge can be defined as the connection between two nodes. If a [[Tree|tree]] has N nodes then it will have (N-1) edges. There is only one path from each node to any other node of the tree.
- Depth of a node: The depth of a node is defined as the length of the path from the root to that node. Each edge adds 1 unit of length to the path. So, it can also be defined as the number of edges in the path from the root of the [[Tree|tree]] to the node.
- Height of a node: The height of a node can be defined as the length of the longest path from the node to a leaf node of the [[Tree|tree]].
- Height of the [[Tree|Tree]]: The height of a [[Tree|tree]] is the length of the longest path from the root of the tree to a leaf node of the tree.
- Degree of a Node: The total count of subtrees attached to that node is called the degree of the node. The degree of a leaf node must be 0. The degree of a [[Tree|tree]] is the maximum degree of a node among all the nodes in the tree.

## Types of trees

- **[[Binary Tree|Binary tree]]**: In a [[Binary Tree|binary tree]], each node can have a maximum of two children linked to it. Some common types of [[Binary Tree|binary trees]] include full binary trees, complete binary trees, balanced binary trees, and degenerate or pathological binary trees.
    - [[Heap Binary Tree|Heap]]: A [[Heap Binary Tree|Heap]] is a complete binary tree data structure that satisfies the heap property: for every node, the value of its children is less than or equal to its own value. Heaps are usually used to implement [[Priority Queue|priority queues]], where the smallest (or largest) element is always at the root of the tree.
- **[[Ternary Tree|Ternary Tree]]**: A Ternary Tree is a tree data structure in which each node has at most three child nodes, usually distinguished as “left”, “mid” and “right”.
- [[N-ary Tree (Generic Tree)|N-ary Tree or Generic Tree]]: Generic trees are a collection of nodes where each node is a data structure that consists of records and a list of references to its children(duplicate references are not allowed). Unlike the linked list, each node stores the address of multiple nodes.
- Trie: The Trie data structure is a tree-like data structure used for storing a dynamic set of strings. It is commonly used for efficient retrieval and storage of keys in a large dataset.

# Graphs

[[Graph|Graph]] Data Structure is a collection of nodes connected by edges. It’s used to represent relationships between different entities. Graph algorithms are methods used to manipulate and analyse graphs, solving various problems like finding the shortest path or detecting cycles.

## Components of a Graph:

- Vertices: Vertices are the fundamental units of the graph. Sometimes, vertices are also known as vertex or nodes. Every node/vertex can be labelled or unlabeled.
- Edges: Edges are drawn or used to connect two nodes of the graph. It can be ordered pair of nodes in a directed graph. Edges can connect any two nodes in any possible way. There are no rules. Sometimes, edges are also known as arcs. Every edge can be labelled/unlabelled.