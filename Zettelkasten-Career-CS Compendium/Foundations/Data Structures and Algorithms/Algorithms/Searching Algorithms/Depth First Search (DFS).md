---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
---
**DFS** explores as far down a branch as possible before backtracking. It uses a **stack** structure, either explicitly (with a stack) or implicitly (via recursion).

# Explanation
- Dives deep before going wide
- Can be used for:
    - Connectivity checking
    - Cycle detection
    - Topological sorting
    - Pathfinding (not guaranteed shortest)
- Needs a **visited** set/map to avoid revisiting nodes (especially for graphs)

## Data Structures
- **Stack** (explicit) or **function call stack** (recursion)
- **Visited set/map** to track explored nodes

# DFS on Trees and Graphs
## DFS on Trees
DFS can be done in three ways:

1. Pre-order (Root → Left → Right)
2. Inorder (Left → Root → Right)
3. Postorder (Left → Right → Root)

### Assumptions
- Binary or N-ary tree
- No cycles, no visited set needed

## DFS on Graphs
### Assumptions
- Graph may have cycles
- Need to track visited nodes
- Can be recursive or iterative (stack)

## Time Complexity
|Context|Time Complexity|
|---|---|
|Tree (N nodes)|O(N)|
|Graph (V vertices, E edges)|O(V + E)|


## State Walkthrough
## Tree
We use this binary tree:

```
		1
       / \
      2   3
     / \   \
    4   5   6
```
### Preorder DFS (Root → Left → Right)

Step-by-Step Execution

- Start from root 1
- Visit 1, go left → 2
- Visit 2, go left → 4
- Visit 4 (no children, backtrack)
- Back at 2, go right → 5
- Visit 5 (no children, backtrack)
- Back at 1, go right → 3
- Visit 3, go right → 6
- Visit 6 (no children, backtrack)

Pre-order Output: 
```
1 → 2 → 4 → 5 → 3 → 6
```

### Inorder DFS (Left → Root → Right)

Step-by-Step Execution

- Start at root 1, go left → 2
- At 2, go left → 4
- Visit 4 (no left, process 4)
- Back to 2, visit 2, go right → 5
- Visit 5 (no children, backtrack)
- Back to 1, visit 1, go right → 3
- At 3, go right → 6
- Visit 6 (no children, backtrack)
- Back to 3, visit 3


Inorder Output: 
```
4 → 2 → 5 → 1 → 3 → 6
```

### Postorder DFS (Left → Right → Root)

Step-by-Step Execution

- Start at root 1, go left → 2
- At 2, go left → 4
- Visit 4 (no children, backtrack)
- Back to 2, go right → 5
- Visit 5 (no children, backtrack)
- Visit 2
- Back at 1, go right → 3
- At 3, go right → 6
- Visit 6 (no children, backtrack)
- Visit 3
- Visit 1


Postorder Output:
```
4 → 5 → 2 → 6 → 3 → 1
```

### DFS Using a Stack (Iterative Approach)

1. Use a stack instead of recursion.
2. Push nodes onto the stack manually.
3. Process nodes in order according to Preorder, Inorder, or Postorder logic.

For Preorder, use a stack:

1. Push root (1).
2. Pop 1, push 3, push 2.
3. Pop 2, push 5, push 4.
4. Pop 4, pop 5.
5. Pop 3, push 6.
6. Pop 6.

Result:
```
1 → 2 → 4 → 5 → 3 → 6
```

## Graph
Adjacency list:

```
1 → 2, 3  
2 → 4  
3 → 5  
5 → 6  
```

|Call Stack|Node Visited|Visited Set|Output|
|---|---|---|---|
|dfs(1)|1|{1}|1|
|→ dfs(2)|2|{1, 2}|2|
|→ → dfs(4)|4|{1, 2, 4}|4|
|→ dfs(3)|3|{1, 2, 3, 4}|3|
|→ → dfs(5)|5|{1, 2, 3, 4, 5}|5|
|→ → → dfs(6)|6|{1, 2, 3, 4, 5, 6}|6|

# Example code
## Tree
### Preorder DFS (Root → Left → Right)
```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}

public class PreorderTraversal {
    public static void preorder(TreeNode root) {
        if (root == null) return;
        System.out.print(root.val + " "); // Visit root
        preorder(root.left);             // Recur left
        preorder(root.right);            // Recur right
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        preorder(root); // Output: 1 2 4 5 3 6
    }
}
```

### Inorder DFS (Left → Root → Right)
```java
public class InorderTraversal {
    public static void inorder(TreeNode root) {
        if (root == null) return;
        inorder(root.left);              // Recur left
        System.out.print(root.val + " "); // Visit root
        inorder(root.right);             // Recur right
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        inorder(root); // Output: 4 2 5 1 3 6
    }
}
```

### Postorder DFS (Left → Right → Root)
```java
public class PostorderTraversal {
    public static void postorder(TreeNode root) {
        if (root == null) return;
        postorder(root.left);            // Recur left
        postorder(root.right);           // Recur right
        System.out.print(root.val + " "); // Visit root
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        postorder(root); // Output: 4 5 2 6 3 1
    }
}
```

### DFS Using a Stack (Iterative Approach)
```java
import java.util.Stack;

class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) {
        this.val = val;
        this.left = this.right = null;
    }
}

public class PreorderStack {
    public static void preorder(TreeNode root) {
        if (root == null) return;

        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop(); // Process the top node
            System.out.print(node.val + " ");

            // Push right child first, so left is processed first
            if (node.right != null) stack.push(node.right);
            if (node.left != null) stack.push(node.left);
        }
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);
        root.right.right = new TreeNode(6);

        preorder(root); // Output: 1 2 4 5 3 6
    }
}
```

## Graph
```java
import java.util.*;

public class GraphDFS {
    public static void dfs(Map<Integer, List<Integer>> graph, int node, Set<Integer> visited) {
        if (visited.contains(node)) return;

        visited.add(node);
        System.out.println(node);

        for (int neighbor : graph.getOrDefault(node, new ArrayList<>())) {
            dfs(graph, neighbor, visited);
        }
    }

    public static void main(String[] args) {
        // Graph:
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

        Set<Integer> visited = new HashSet<>();
        dfs(graph, 1, visited);
    }
}
```

```java
public class IterativeGraphDFS {
    public static void dfs(Map<Integer, List<Integer>> graph, int start) {
        Set<Integer> visited = new HashSet<>();
        Stack<Integer> stack = new Stack<>();

        stack.push(start);

        while (!stack.isEmpty()) {
            int node = stack.pop();

            if (visited.contains(node)) continue;

            visited.add(node);
            System.out.println(node);

            // Push neighbors in reverse order to preserve natural order
            List<Integer> neighbors = graph.getOrDefault(node, new ArrayList<>());
            Collections.reverse(neighbors);
            for (int neighbor : neighbors) {
                if (!visited.contains(neighbor)) {
                    stack.push(neighbor);
                }
            }
        }
    }
}
```

