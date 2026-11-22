---
tags:
  - MOC
---
This note serves as a centralized reference, linking to your individual notes on common graph algorithms.
## Search Algorithms

• [[Breadth-First Search (BFS)]]: Explores all the neighboring nodes at the present depth before moving on to the nodes at the next depth level. Best for finding the shortest path in terms of the number of edges.  
• [[Depth First Search (DFS)]]: Explores as far as possible along each branch before backtracking. Useful for topological sorting and finding connected components.  
## Shortest Path Algorithms

• [[Dijkstra’s Algorithm]]: Finds the shortest paths from a single source node to all other nodes in a graph with non-negative edge weights.  
• [[Bellman-Ford]]: Finds the shortest paths from a single source node to all other nodes, even in graphs with negative edge weights. It can also detect the presence of negative cycles.  
• [[Floyd-Warshall]]: Finds the shortest paths between all pairs of nodes in a graph.  
  
## Minimum Spanning Tree (MST) Algorithms

• [[Kruskal’s and Prim’s Algorithm (MST)]]: Finds a Minimum Spanning Tree for a weighted undirected graph by selecting the edge with the smallest weight that does not form a cycle.  
## Specific Algorithms & Data Structures

•[[A(star) Algorithm]]: An informed search algorithm that uses a heuristic to guide its search, making it an extension of Dijkstra's that performs better in practice for pathfinding (e.g., in video games or route planning).  
• [[Union-Find and Disjoint Set Union (DSU)]]: A data structure that manages a collection of disjoint sets. It is primarily used to check if two elements are in the same set and for efficiently implementing Kruskal's algorithm.