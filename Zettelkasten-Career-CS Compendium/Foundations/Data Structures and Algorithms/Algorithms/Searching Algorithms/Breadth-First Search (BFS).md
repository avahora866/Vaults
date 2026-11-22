---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
---
**BFS** is a traversal algorithm that explores all neighbors at the current depth before moving to the next level. It uses a **queue** data structure to manage the exploration order.

# Explanation
- Explores nodes level by level
- Guarantees the **shortest path in unweighted graphs**
- Requires a way to track visited nodes to avoid cycles (for graphs)

## Data Structures
- **Queue**: for managing the order of nodes to visit
- **Visited Set**: for marking nodes that have already been explored

## Tree
### Assumptions
- Rooted tree (no cycles)
- Binary or N-ary tree structure

## Graph
### Assumptions
- Undirected or directed graph
- May contain cycles → must track visited nodes

## Time Complexity
|Context|Time Complexity|
|---|---|
|Tree (N nodes)|O(N)|
|Graph (V vertices, E edges)|O(V + E)|

This assumes the graph is represented with an adjacency list.

## State Walkthrough
### Tree
We have this binary tree:

```
		1
       / \
      2   3
     / \   \
    4   5   6
```

#### Step 1: Initialize the Queue
- Start BFS from the root node (1).
- Enqueue root (1).

Queue State:
```
[1]
```

#### Step 2: Process Node 1

- Dequeue 1 (remove from front).
- Visit 1 (process it).
- Enqueue children 2 and 3.

Queue State:
```
[2, 3]
```

#### Step 3: Process Node 2

- Dequeue 2.
- Visit 2.
- Enqueue children 4 and 5.

Queue State: 
```
[3, 4, 5]
```
#### Step 4: Process Node 3

- Dequeue 3.
- Visit 3.
- Enqueue child 6 (no left child).

Queue State:
```
[4, 5, 6]
```
#### Step 5: Process Node 4

- Dequeue 4.
- Visit 4 (no children to enqueue).

Queue State: 
```
[5, 6]
```
#### Step 6: Process Node 5

- Dequeue 5.
- Visit 5 (no children to enqueue).

Queue State: 
```
[6]
```
#### Step 7: Process Node 6

- Dequeue 6.
- Visit 6 (no children to enqueue).

Queue State: 
```
[] (empty)
```

#### BFS Order (Final Output)

```
1 → 2 → 3 → 4 → 5 → 6
```

### Graph
Adjacency list:

```
1 → 2, 3  
2 → 4  
3 → 5  
5 → 6  
```

|Queue Contents|Node Processed|Visited|Output|
|---|---|---|---|
|[1]|1|{1}|1|
|[2, 3]|2|{1, 2}|2|
|[3, 4]|3|{1, 2, 3}|3|
|[4, 5]|4|{1, 2, 3, 4}|4|
|[5]|5|{1, 2, 3, 4, 5}|5|
|[6]|6|{1, 2, 3, 4, 5, 6}|6|
|[]|-|-|-|

# Example code
## Tree
```java
import java.util.*;

class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}

public class TreeBFS {
    public static void bfs(TreeNode root) {
        if (root == null) return;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            System.out.println(node.val);

            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }

    public static void main(String[] args) {
        // Tree:     1
        //         /   \
        //        2     3
        //       / \   / \
        //      4   5 6   7
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.left = new TreeNode(6);
        root.right.right = new TreeNode(7);

        bfs(root);
    }
}
```

## Graph
```java
import java.util.*;

public class GraphBFS {
    public static void bfs(Map<Integer, List<Integer>> graph, int start) {
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();

        queue.offer(start);
        visited.add(start);

        while (!queue.isEmpty()) {
            int node = queue.poll();
            System.out.println(node);

            for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {
        // Graph (adjacency list):
        // 1: [2, 3]
        // 2: [4]
        // 3: [5]
        // 4: []
        // 5: [6]
        // 6: []
        Map<Integer, List<Integer>> graph = new HashMap<>();
        graph.put(1, Arrays.asList(2, 3));
        graph.put(2, Arrays.asList(4));
        graph.put(3, Arrays.asList(5));
        graph.put(5, Arrays.asList(6));

        bfs(graph, 1);
    }
}
```
