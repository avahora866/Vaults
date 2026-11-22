---
tags:
  - DataStructure
---
**Union-Find** or **Disjoint Set Union (DSU)**, which is a powerful data structure used to efficiently handle **dynamic connectivity problems**. It's especially useful in problems where you need to manage and merge sets of elements and frequently check if two elements belong to the same set.

---

# What is Union-Find (Disjoint Set Union)?

The **Union-Find** (or **Disjoint Set Union**, DSU) is a data structure that supports two primary operations efficiently:

1. **Union**: Combine two sets into one.

2. **Find**: Determine which set a particular element is in.

In addition, it supports two key optimizations:

* **Path Compression**: A technique to flatten the structure of the tree when performing `find`, which helps in speeding up future operations.

* **Union by Rank/Size**: A technique to keep the tree balanced by always attaching the smaller tree under the root of the larger tree when performing a `union`.

These optimizations make **Union-Find** an efficient solution for problems involving connected components or connectivity queries in dynamic graphs.

---

# Core Operations

## Find

The `find` operation determines which set an element belongs to. This is done by traversing up the tree of the set, eventually reaching the "root" of the tree (i.e., the representative element of the set).

Without any optimization, the `find` operation can take linear time, but using **path compression**, we can make each subsequent `find` operation much faster.

## Union

The `union` operation merges two sets. If two elements belong to different sets, they are merged into a single set. Using **union by rank/size**, we can attach the smaller tree to the larger tree, keeping the tree balanced and reducing its height.

---

# Data Structure Representation

The Union-Find data structure typically uses two arrays:

1. **Parent Array**: This array holds the parent of each element. If an element is its own parent, it’s the root of the set.

2. **Rank/Size Array**: This array holds the rank (or size) of the set, which is used to keep the tree balanced during union operations.

---

# How Union-Find Works:

1. **Initialization**: Start with each element in its own set (each element is its own parent, and the rank or size is 1).

2. **Find Operation**: Check the root of the element’s set, compressing the path by pointing each node directly to the root.

3. **Union Operation**: Merge the sets of two elements. If the elements belong to different sets, use union by rank or size to decide which set becomes the root.

---

# Example:

Consider a set of elements {0, 1, 2, 3, 4}.

## Initial State:

```
Parent Array: [0, 1, 2, 3, 4]
Rank Array:   [0, 0, 0, 0, 0]   (or Size Array: [1, 1, 1, 1, 1])
```

Each element is its own parent, so each element is in its own set.

## Union Operation: `union(0, 1)`

After calling `union(0, 1)`, 0 and 1 are merged into one set. Since both elements have the same rank, we can attach 1 as the child of 0:

```
Parent Array: [0, 0, 2, 3, 4]
Rank Array:   [1, 0, 0, 0, 0]  (Rank of 0 increased)
```

## Union Operation: `union(1, 2)`

Now, we merge 1 and Since 1 is already part of the set with 0, we attach 2 to 0:

```
Parent Array: [0, 0, 0, 3, 4]
Rank Array:   [1, 0, 0, 0, 0]
```

## Find Operation: `find(2)`

The `find(2)` operation will return 0, since 2 is in the set with 0\. With **path compression**, we can optimize the tree structure, making 2 directly point to 0, reducing future search time.

```
Parent Array: [0, 0, 0, 3, 4]
Rank Array:   [1, 0, 0, 0, 0]
```

---

# Optimizations

## Path Compression

When performing a `find` operation, we can make the tree flatter by making every node in the path point directly to the root. This speeds up future `find` operations.

Example: If `find(2)` traverses from 2 → 0 → 1 → root, path compression will update the parent array so that each node directly points to the root:

```
Before Path Compression:
Parent Array: [0, 0, 1, 3, 4]

After Path Compression (find(2)):
Parent Array: [0, 0, 0, 3, 4]
```

## Union by Rank/Size

When merging two sets, the `union` operation uses the **rank** (or **size**) of the sets to decide which tree becomes the root. The smaller tree is attached to the larger tree to ensure that the trees remain as flat as possible.

* **Union by Rank**: Attach the tree with the smaller rank under the tree with the larger rank.

* **Union by Size**: Attach the tree with the smaller size (number of elements) under the tree with the larger size.

---

# Java Code Implementation of Union-Find with Path Compression and Union by Rank:

```java
public class UnionFind {

    private int[] parent;
    private int[] rank;

    // Constructor to initialize the parent and rank arrays
    public UnionFind(int size) {
        parent = new int[size];
        rank = new int[size];
        
        // Initially, each node is its own parent, and rank is 0
        for (int i = 0; i < size; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }

    // Find the root of the set containing 'x' with path compression
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }

    // Union two sets that contain 'x' and 'y' using union by rank
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);

        if (rootX != rootY) {
            // Union by rank: attach the smaller tree under the larger one
            if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++; // Increase the rank
            }
        }
    }

    // Check if two elements are in the same set
    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }

    // For debugging, print the parent array and rank array
    public void print() {
        System.out.println("Parent Array: " + java.util.Arrays.toString(parent));
        System.out.println("Rank Array: " + java.util.Arrays.toString(rank));
    }

    public static void main(String[] args) {
        UnionFind uf = new UnionFind(5);

        uf.union(0, 1);
        uf.union(1, 2);
        uf.union(3, 4);
        
        uf.print();

        // Check if 0 and 2 are connected (should be true)
        System.out.println("Are 0 and 2 connected? " + uf.connected(0, 2));

        // Check if 0 and 4 are connected (should be false)
        System.out.println("Are 0 and 4 connected? " + uf.connected(0, 4));
    }
}
```

---

## Explanation:

* **find(x)**: Returns the root of the set containing `x`, using path compression to speed up future queries.

* **union(x, y)**: Merges the sets containing `x` and `y` using union by rank to minimize tree height.

* **connected(x, y)**: Returns `true` if `x` and `y` are in the same set (i.e., have the same root).

* **print()**: For debugging purposes, prints the current state of the parent and rank arrays.

---

# Time Complexity

* **Find**: The time complexity of `find(x)` with path compression is nearly **O(α(N))**, where `α(N)` is the **inverse Ackermann function**, which grows extremely slowly. In practice, this is very close to **O(1)**.

* **Union**: The time complexity of `union(x, y)` with union by rank/size is also **O(α(N))**, which is close to **O(1)**.

* For `N` elements, both operations are **very efficient**, nearly constant time in practice.

---

# Applications of Union-Find

* **Connected Components in a Graph**: Helps in detecting connected components in a graph, which is useful for algorithms like Kruskal's algorithm for Minimum Spanning Tree (MST).

* **Cycle Detection in Graphs**: Union-Find can be used to detect cycles in an undirected graph.

* **Network Connectivity**: In networking, to manage and merge sets of connected computers or nodes.

---

# Summary

| Feature | Description |
| ----- | ----- |
| Operations | **Find** and **Union** |
| Optimizations | **Path Compression** and **Union by Rank/Size** |
| Time Complexity | **O(α(N))** per operation, nearly constant |
| Space Complexity | **O(N)** for the parent and rank arrays |
| Applications | Cycle detection, Kruskal's MST, network connectivity, etc. |

---

