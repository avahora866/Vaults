---
tags:
  - Algorithm
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
**Bellman-Ford Algorithm**, which is another important algorithm for solving the **Single Source Shortest Path** problem, but with a crucial difference from Dijkstra’s algorithm: **Bellman-Ford can handle graphs with negative edge weights**.

# Explanation
* It handles graphs with **negative weight edges**.
* It can detect **negative weight cycles** (cycles where the total weight of the cycle is negative, leading to an infinite loop of reducing distance).
* It works by **relaxing** all edges **V-1 times**, where `V` is the number of vertices in the graph.

## Relaxation Process:
1. **Relax an edge**: If the current path to a node is longer than a path through an adjacent node, we update the shortest distance.
2. Repeat the relaxation process for all edges `V-1` times, where `V` is the number of vertices.
3. The reason we need `V-1` iterations is that the longest possible shortest path in a graph without negative cycles can have at most `V-1` edges.

* **Negative weight cycles detection**: After completing `V-1` relaxations, we perform an extra iteration to check if any edge can still be relaxed. If we can relax an edge, then it means the graph has a **negative weight cycle**. This is important for applications where you need to ensure there are no such cycles in the graph, like in financial transactions or routing algorithms.

## When to Use Bellman-Ford?
* **Graphs with negative weights**: Dijkstra won't work with negative weights, but Bellman-Ford can handle them.
* **Cycle Detection**: If you're specifically looking to **detect negative weight cycles**, Bellman-Ford is ideal.
* **Smaller graphs**: For small graphs where the time complexity of O(V \* E) is acceptable.

## Time Complexity
**O(V \* E)**: We perform `V-1` relaxations, and each relaxation step processes `E` edges. So, the overall time complexity is **O(V \* E)**.
This is slower than Dijkstra’s algorithm (O((V \+ E) log V)), but Bellman-Ford can handle negative weight edges and negative weight cycles.

## State Walkthrough
#TODO

# Example code
```java
import java.util.*;

class Edge {
    int u, v, weight;
    Edge(int u, int v, int weight) {
        this.u = u;
        this.v = v;
        this.weight = weight;
    }
}

public class BellmanFord {

    public static boolean bellmanFord(int V, List<Edge> edges, int src) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        // Relax all edges V-1 times
        for (int i = 1; i < V; i++) {
            for (Edge edge : edges) {
                if (dist[edge.u] != Integer.MAX_VALUE && dist[edge.u] + edge.weight < dist[edge.v]) {
                    dist[edge.v] = dist[edge.u] + edge.weight;
                }
            }
        }

        // Check for negative-weight cycles
        for (Edge edge : edges) {
            if (dist[edge.u] != Integer.MAX_VALUE && dist[edge.u] + edge.weight < dist[edge.v]) {
                System.out.println("Graph contains negative weight cycle");
                return false;
            }
        }

        // Print the shortest distance
        System.out.println("Shortest distances from source " + src + ":");
        for (int i = 0; i < V; i++) {
            System.out.println("To node " + i + ": " + (dist[i] == Integer.MAX_VALUE ? "Infinity" : dist[i]));
        }

        return true;
    }

    public static void main(String[] args) {
        int V = 5; // Number of vertices
        List<Edge> edges = new ArrayList<>();
        
        // Add edges (u, v, weight)
        edges.add(new Edge(0, 1, -1));
        edges.add(new Edge(0, 2, 4));
        edges.add(new Edge(1, 2, 3));
        edges.add(new Edge(1, 3, 2));
        edges.add(new Edge(1, 4, 2));
        edges.add(new Edge(3, 2, 5));
        edges.add(new Edge(3, 1, 1));
        edges.add(new Edge(4, 3, -3));

        // Run Bellman-Ford algorithm
        bellmanFord(V, edges, 0);  // 0 is the source vertex
    }
}
```
## Explanation of Code:

* **Edge class**: Defines an edge with a start node `u`, an end node `v`, and a weight `weight`.

* **bellmanFord method**:

  * Initializes the `dist` array where `dist[i]` holds the shortest distance from the source node to node `i`.

  * It then performs **V-1 relaxations** (the core of Bellman-Ford).

  * Finally, it checks if any edge can still be relaxed — which would indicate a **negative weight cycle**.

* The algorithm terminates with either the shortest path distances or an error if a negative cycle is detected.

## Example of Negative Weight Cycle:

```java
// In this graph, there is a negative weight cycle.
edges.add(new Edge(0, 1, 1));
edges.add(new Edge(1, 2, -1));
edges.add(new Edge(2, 0, -1)); // This forms a negative cycle: 0 -> 1 -> 2 -> 0
```
