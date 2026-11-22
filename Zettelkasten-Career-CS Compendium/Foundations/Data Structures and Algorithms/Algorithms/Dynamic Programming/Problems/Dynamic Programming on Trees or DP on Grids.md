---
tags:
  - Algorithm
---
# DP on Grids

## When to Use:

When you're solving a problem on a **2D matrix/grid** where movement is usually constrained (right/down, etc.) and you need to **accumulate values**.

---

## Example Problem: Minimum Path Sum

**Problem:** Given a `m x n` grid filled with non-negative numbers, find a path from top-left to bottom-right with **minimum sum**. You can only move **right** or **down**.

## Java Code:

```java
public class GridDP {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];

        // fill first row
        for (int j = 1; j < n; j++)
            dp[0][j] = dp[0][j - 1] + grid[0][j];

        // fill first column
        for (int i = 1; i < m; i++)
            dp[i][0] = dp[i - 1][0] + grid[i][0];

        // fill rest of grid
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }

        return dp[m - 1][n - 1];
    }
}
```

---

## Time & Space:

* Time: O(m × n)

* Space: O(m × n), can be optimized to O(n)

---

# DP on Trees

## When to Use:

When you're dealing with **hierarchical data (tree)** and need to solve a problem for **each node**, based on its children.

---

## Example Problem: House Robber III (Leetcode 337\)

**Problem:** Each tree node has a value. If you rob a node, you can't rob its children. Return the maximum amount you can rob.

## Approach:

Use **post-order DFS** and for each node return:

* `robbed` → max value if you rob this node

* `notRobbed` → max value if you skip this node

---

## Java Code:

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) { val = x; }
}

public class TreeDP {

    public int rob(TreeNode root) {
        int[] result = dfs(root);
        return Math.max(result[0], result[1]);
    }

    private int[] dfs(TreeNode node) {
        if (node == null) return new int[]{0, 0};

        int[] left = dfs(node.left);
        int[] right = dfs(node.right);

        int rob = node.val + left[1] + right[1];      // Rob this node: can't rob children
        int notRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]); // Don't rob: take max of children

        return new int[]{rob, notRob};
    }
}
```

---

## Time & Space:

* Time: O(n), where n is number of nodes

* Space: O(h), recursion stack (h \= height of tree)

---

# Summary

| Type | Structure | Movement | State Depends On | Example Problem |
| ----- | ----- | ----- | ----- | ----- |
| DP Grid | 2D Matrix | Right, Down | Previous cells (top/left) | Min Path Sum, Unique Paths |
| DP Tree | Tree | Post-order DFS | Child nodes | House Robber III, Diameter |



