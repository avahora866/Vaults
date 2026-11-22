---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
**Floyd-Warshall Algorithm**, which is another classic algorithm used to solve the **All-Pairs Shortest Path** problem in graphs. It computes the shortest paths between **all pairs of nodes** in a graph.

# Explanation
The **Floyd-Warshall Algorithm** computes the shortest paths between all pairs of vertices in a **weighted graph** (either directed or undirected). It is a dynamic programming algorithm that works on both **positive and negative weights**, and it also **detects negative weight cycles**.

* **All-Pairs Shortest Path**: Unlike algorithms like Dijkstra and Bellman-Ford, which find the shortest path from a single source to all other nodes, **Floyd-Warshall computes shortest paths between every pair of nodes**.
* It works for both **directed and undirected graphs**.
* It can handle graphs with **negative weights** and **detect negative weight cycles**.
* The algorithm is relatively simple but not the most efficient for sparse graphs due to its time complexity.

## How Does it Work?
1. **Initialization**: Create a distance matrix `dist[][]`, where `dist[i][j]` is the initial direct edge weight from node `i` to node `j`. If there is no direct edge, set the distance to infinity.
2. **Dynamic Programming Update**: For each node `k`, update the shortest path between all pairs `(i, j)` by checking if a path through `k` gives a shorter distance:
```
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```
3.   
   This checks if going from node `i` to `j` through node `k` gives a shorter path than the direct path from `i` to `j`.
4. Repeat the above step for all nodes as intermediate nodes.
5. After `V` iterations (where `V` is the number of vertices), `dist[i][j]` will contain the shortest distance between node `i` and node `j`.

## Negative Weight Cycle Detection
If there is a negative weight cycle in the graph, the Floyd-Warshall algorithm can detect it by checking if `dist[i][i]` becomes negative for any vertex `i`. If `dist[i][i] < 0`, it means that vertex `i` is part of a negative weight cycle.

For example, consider the following graph with a negative cycle:

```
0 → 1 (weight 1)
1 → 2 (weight -1)
2 → 0 (weight -1)
```

After running the algorithm, `dist[0][0]`, `dist[1][1]`, and `dist[2][2]` will be negative, indicating the presence of a negative cycle.

## When to Use Floyd-Warshall?
* **All-pairs shortest path**: If you need the shortest paths between all pairs of nodes.
* **Graphs with negative weights**: It can handle graphs with negative edge weights, and also detects negative weight cycles.
* **Small to medium-sized graphs**: Since its time complexity is **O(V³)**, it's suitable for graphs with a relatively small number of vertices (e.g., fewer than a few thousand).

## Time Complexity
* **O(V³)**, where `V` is the number of vertices in the graph.
* The algorithm requires three nested loops, each iterating over the vertices.

## Space Complexity
* **O(V²)**, since we store a 2D matrix `dist[][]` of size `V × V`.

## State Walkthrough
#TODO

# Example code
```java
public class FloydWarshall {

    // Method to implement Floyd-Warshall algorithm
    public static void floydWarshall(int[][] graph) {
        int V = graph.length;
        
        // dist[][] will be the output matrix that will store the shortest distances
        int[][] dist = new int[V][V];
        
        // Initialize the distance array with graph's adjacency matrix
        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (graph[i][j] == 0 && i != j) {
                    dist[i][j] = Integer.MAX_VALUE;  // No path exists
                } else {
                    dist[i][j] = graph[i][j];
                }
            }
        }

        // Floyd-Warshall algorithm
        for (int k = 0; k < V; k++) {
            for (int i = 0; i < V; i++) {
                for (int j = 0; j < V; j++) {
                    if (dist[i][k] != Integer.MAX_VALUE && dist[k][j] != Integer.MAX_VALUE) {
                        dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }

        // Print the result
        printSolution(dist);
    }

    // Utility method to print the distance matrix
    public static void printSolution(int[][] dist) {
        System.out.println("Shortest distances between every pair of vertices:");
        for (int i = 0; i < dist.length; i++) {
            for (int j = 0; j < dist.length; j++) {
                if (dist[i][j] == Integer.MAX_VALUE) {
                    System.out.print("∞ ");  // No path
                } else {
                    System.out.print(dist[i][j] + " ");
                }
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        // Example graph in adjacency matrix format
        int[][] graph = {
            {0, 5, Integer.MAX_VALUE, 10},
            {Integer.MAX_VALUE, 0, 3, Integer.MAX_VALUE},
            {Integer.MAX_VALUE, Integer.MAX_VALUE, 0, 1},
            {Integer.MAX_VALUE, Integer.MAX_VALUE, Integer.MAX_VALUE, 0}
        };
        
        // Running the Floyd-Warshall algorithm
        floydWarshall(graph);
    }
}
```
## Explanation of the Code:
1. **Initialization**: The graph is provided as an adjacency matrix, where `graph[i][j]` represents the weight of the edge from node `i` to node `j`. If there is no direct edge, it's represented as `Integer.MAX_VALUE`.
2. **Floyd-Warshall Core Logic**:
   * We iterate over every node as an intermediate node (`k`), and for each pair of nodes `(i, j)`, we update the shortest distance between `i` and `j` by checking if a shorter path exists through node `k`.
3. **Result Output**: The final `dist[][]` matrix contains the shortest distances between all pairs of nodes, and we print it.

## Example Output:
```
Shortest distances between every pair of vertices:
0 5 8 9 
∞ 0 3 6 
∞ ∞ 0 1 
∞ ∞ ∞ 0 
```
*   
  This matrix tells us the shortest distance between every pair of nodes. For example:
  * The shortest path from node `0` to node `2` is `8` (via node `1`).
  * There is no path from node `1` to node `0`, so it's marked as `∞`.
