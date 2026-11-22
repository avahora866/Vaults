---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
#TODO

# Explanation
**Kruskal’s Algorithm** is a **greedy algorithm** that finds the MST by considering edges in increasing order of their weights. Here's how it works:

## How it works:
1. **Sort all edges** in the graph by weight in **non-decreasing** order.
2. Initialize an empty set to store the edges of the MST.
3. For each edge (from the sorted list):
   * If including the edge does not form a cycle (i.e., the two vertices are not already connected), add the edge to the MST.
   * Otherwise, discard the edge.
4. Repeat until the MST contains exactly **V-1 edges** (where `V` is the number of vertices).

## Key Data Structures:
* **Disjoint Set (Union-Find)**: To check whether adding an edge forms a cycle. We use the **Union-Find (Disjoint Set Union)** data structure to track which vertices are already connected.
* **Priority Queue/Min-Heap (optional)**: For sorting edges by weight.

## Time Complexity
**O(E log E)**, where `E` is the number of edges. Sorting the edges takes **O(E log E)**, and each `union/find` operation takes **O(α(V))** (inverse Ackermann function) time.

## State Walkthrough
Consider a graph with vertices `{A, B, C, D}` and the following edges with weights:

```
Edges:
A-B: 1
A-C: 4
B-C: 2
B-D: 5
C-D: 3
```

* Step 1: Sort edges by weight: `(A-B: 1)`, `(B-C: 2)`, `(C-D: 3)`, `(A-C: 4)`, `(B-D: 5)`.
* Step 2: Add edges to the MST, checking for cycles with Union-Find:
  * Add edge A-B (weight 1).
  * Add edge B-C (weight 2).
  * Add edge C-D (weight 3).
  * Stop here since the MST already has `V-1 = 4-1 = 3` edges.

**Resulting MST:**
* Edges in MST: `(A-B: 1)`, `(B-C: 2)`, `(C-D: 3)`.
* Total weight \= 1 \+ 2 \+ 3 \= 6\.


# Example code
```java
import java.util.*;

public class KruskalMST {
    static class Edge {
        int src, dest, weight;
        Edge(int src, int dest, int weight) {
            this.src = src;
            this.dest = dest;
            this.weight = weight;
        }
    }

    static class UnionFind {
        int[] parent, rank;

        UnionFind(int n) {
            parent = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                rank[i] = 0;
            }
        }

        int find(int x) {
            if (parent[x] != x)
                parent[x] = find(parent[x]);
            return parent[x];
        }

        void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX != rootY) {
                if (rank[rootX] > rank[rootY]) {
                    parent[rootY] = rootX;
                } else if (rank[rootX] < rank[rootY]) {
                    parent[rootX] = rootY;
                } else {
                    parent[rootY] = rootX;
                    rank[rootX]++;
                }
            }
        }
    }

    public static int kruskalMST(List<Edge> edges, int V) {
        Collections.sort(edges, Comparator.comparingInt(e -> e.weight));
        UnionFind uf = new UnionFind(V);
        int mstWeight = 0;
        
        for (Edge e : edges) {
            int rootSrc = uf.find(e.src);
            int rootDest = uf.find(e.dest);
            if (rootSrc != rootDest) {
                uf.union(rootSrc, rootDest);
                mstWeight += e.weight;
                System.out.println("Edge: " + e.src + "-" + e.dest + " with weight " + e.weight);
            }
        }
        return mstWeight;
    }

    public static void main(String[] args) {
        List<Edge> edges = new ArrayList<>();
        edges.add(new Edge(0, 1, 1));
        edges.add(new Edge(0, 2, 4));
        edges.add(new Edge(1, 2, 2));
        edges.add(new Edge(2, 3, 3));
        edges.add(new Edge(1, 3, 5));

        int V = 4;  // Number of vertices
        int mstWeight = kruskalMST(edges, V);
        System.out.println("Total weight of MST: " + mstWeight);
    }
}
```
