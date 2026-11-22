---
tags:
  - FLASHCARD
  - DataStructure
---
A graph is a non-linear data structure consisting of nodes (or vertices) and edges that connect these nodes. Graphs are used to represent relationships between different objects.

# Key Components

1. **Nodes (Vertices)**: These are the fundamental units of a graph, representing entities.

2. **Edges**: These are the connections between nodes, representing relationships between entities.

# Types of Graphs

- **Undirected Graph**: Edges have no direction. If node A is connected to node B, then node B is also connected to node A.

- **Directed Graph (Digraph)**: Edges have direction. An edge from node A to node B doesn't imply an edge from B to A.

- **Weighted Graph**: Edges have weights or costs associated with them.

- **Unweighted Graph**: All edges have the same weight or no weight.

- **Cyclic Graph**: Contains at least one cycle (a path that starts and ends at the same node).

- **Acyclic Graph**: Contains no cycles.

# Common Representations

1. **Adjacency Matrix**: A 2D array where matrix[i][j] represents an edge from node i to node j.
   - Quick to check if an edge exists between two nodes: O(1)
   - Space inefficient for sparse graphs: O(V²)

2. **Adjacency List**: An array or hashmap of linked lists, where each list contains the neighbors of a node.
   - Space efficient for sparse graphs: O(V+E)
   - Slower for checking if an edge exists: O(degree of node)

# Common Graph Algorithms

- **Breadth-First Search (BFS)**: Explores all neighbors at the current depth before moving to nodes at the next depth level.

- **Depth-First Search (DFS)**: Explores as far as possible along a branch before backtracking.

- **Dijkstra's Algorithm**: Finds the shortest path from a source node to all other nodes in a weighted graph.

- **Minimum Spanning Tree Algorithms** (Prim's, Kruskal's): Find a subset of edges that connect all nodes with the minimum total edge weight.

# Real-world Applications

- Social networks (friends connections)
- Road networks and navigation systems
- Network routing
- Dependency resolution in software packages
- Web page linking structures