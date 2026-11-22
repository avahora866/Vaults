---
tags:
  - FLASHCARD
  - DataStructure
---
A **heap** is a specialized binary tree-based data structure that satisfies the **heap property**. It is commonly used in scenarios where quick access to the smallest (or largest) element is required, such as in priority queues or algorithms like [[Data Structures & Algorithms/Algorithms/2. Merge Algorithms/K-Way Merge|K-way Merge]].
# Heap Property
The heap property can be one of two:
1. **Min-Heap Property:** The value of every parent node is less than or equal to its children.
2. **Max-Heap Property:** The value of every parent node is greater than or equal to its children.
You cannot define a custom property for the structure to follow as it would no longer be a heap binary tree
# Binary Tree
A heap is a complete binary tree, meaning all levels of the tree are fully filled except possibly the last one, which is filled from left to right.
# Heap Operations

Heaps are often implemented using arrays for efficiency. Common operations include:
1. Insertion:
	- Insert the new element at the end of the heap (array).
	- "Bubble up" (or sift up) the element by swapping it with its parent until the heap property is restored.
1. Extraction:
    - Remove the root element (smallest in a Min-Heap or largest in a Max-Heap).
    - Replace the root with the last element in the heap.
    - "Bubble down" (or sift down) the element by swapping it with its smallest (or largest) child until the heap property is restored.
3. Peek:
    - Access the root element (minimum or maximum) in O(1)O(1)O(1) time.
4. Heapify:
    - Convert an arbitrary array into a heap by applying the heap property iteratively.

## Time Complexity
- **Insertion:** O(log⁡n)O(\log n)O(logn)
- **Extraction:** O(log⁡n)O(\log n)O(logn)
- **Peek:** O(1)O(1)O(1)
- **Heapify:** O(n)O(n)O(n) (bulk operation)
# Heap Applications
- **Priority Queues:** Store elements with associated priorities for efficient retrieval.
- [[Data Structures & Algorithms/Algorithms/2. Merge Algorithms/K-Way Merge|K-way Merge]]: Efficiently track the smallest (or largest) element across multiple sorted lists.
- **Heap Sort:** A sorting algorithm that uses a heap to repeatedly extract the smallest or largest element.
- **Graph Algorithms:** Dijkstra’s and Prim’s algorithms rely on heaps for efficient edge or vertex selection.
# Example of Min-Heap

Given an array [4, 10, 3, 5, 1], we construct a Min-Heap:

1. Start with the first element: [4].
2. Add 10: [4, 10].
3. Add 3 and bubble up: [3, 10, 4].
4. Add 5: [3, 5, 4, 10].
5. Add 1 and bubble up: [1, 3, 4, 10, 5].

```
        1
      /   \
     3     4
    / \
   10  5

```
# Summary
In summary, a heap is specialised version of a binary tree where it follows one of two heap properteries:
1. **Min-Heap Property:** The value of every parent node is less than or equal to its children.
2. **Max-Heap Property:** The value of every parent node is greater than or equal to its children.