---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
#TODO

# Explanation
**Prim’s Algorithm** is another **greedy algorithm** for finding the MST. It works by growing the MST one vertex at a time, starting from an arbitrary vertex. It always picks the **minimum weight edge** that connects a vertex inside the MST to a vertex outside the MST.

## How it works:
1. Start with an arbitrary vertex and add it to the MST.
2. Add the minimum weight edge that connects a vertex in the MST to a vertex outside the MST.
3. Repeat step 2 until all vertices are included in the MST.

## Key Data Structures:
* **Priority Queue (Min-Heap)**: To efficiently get the minimum weight edge at each step.
* **Adjacency List/Matrix**: To represent the graph.

## Time Complexity
* **O(E log V)**, where `E` is the number of edges and `V` is the number of vertices. Using a priority queue (min-heap), each insertion and extraction of the minimum edge takes **O(log V)** time.

## State Walkthrough
#TODO

# Example code
#TODO