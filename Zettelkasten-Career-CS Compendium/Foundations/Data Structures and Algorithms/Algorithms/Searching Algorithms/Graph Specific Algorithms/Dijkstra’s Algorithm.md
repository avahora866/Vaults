---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
 **Dijkstra’s Algorithm** — a fundamental algorithm used to find the **shortest path from a single source** to all other nodes in a **weighted, non-negative graph**.

# Explanation
* Works only when **all edge weights are non-negative**.
* Uses a **priority queue (min-heap)** to greedily pick the next closest node.
* If you need the actual shortest **path**, you can maintain a `parent[]` array to backtrack.
* Dijkstra doesn't work with **negative weights** — use **Bellman-Ford** for that.

## When to Use Dijkstra
* Weighted graph with **non-negative edges**
* Need **shortest path** from a **single source**
* Common in **maps**, **routing**, and **network optimization**

## Core Idea (Step-by-step):
1. Start with a distance array, initialized to infinity.
2. Distance to source node is 0\.
3. Use a **min-heap (priority queue)** to always pick the node with the **shortest current known distance**.
4. For each neighbor of the current node, check if a **shorter path exists**.
5. If yes, **update the distance** and push it into the heap.

## Time Complexity
* Using Min Heap (PriorityQueue): **O((V \+ E) log V)**
  * V \= number of vertices
  * E \= number of edges

## State Walkthrough
#TODO

# Example code
## PriorityQueue
```java
import java.util.*;

class Pair {
    int node, dist;
    Pair(int node, int dist) {
        this.node = node;
        this.dist = dist;
    }
}

public class Dijkstra {

    public static int[] dijkstra(int V, List<List<Pair>> adj, int src) {
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;

        PriorityQueue<Pair> pq = new PriorityQueue<>(Comparator.comparingInt(p -> p.dist));
        pq.offer(new Pair(src, 0));

        while (!pq.isEmpty()) {
            Pair current = pq.poll();
            int u = current.node;

            for (Pair neighbor : adj.get(u)) {
                int v = neighbor.node;
                int weight = neighbor.dist;

                if (dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.offer(new Pair(v, dist[v]));
                }
            }
        }

        return dist;
    }

    public static void main(String[] args) {
        int V = 5;
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) adj.add(new ArrayList<>());

        // Graph: u -> v (weight)
        adj.get(0).add(new Pair(1, 2));
        adj.get(0).add(new Pair(2, 4));
        adj.get(1).add(new Pair(2, 1));
        adj.get(1).add(new Pair(3, 7));
        adj.get(2).add(new Pair(4, 3));
        adj.get(3).add(new Pair(4, 1));

        int[] distances = dijkstra(V, adj, 0);

        System.out.println("Shortest distances from node 0:");
        for (int i = 0; i < V; i++) {
            System.out.println("To node " + i + ": " + distances[i]);
        }
    }
}
```
