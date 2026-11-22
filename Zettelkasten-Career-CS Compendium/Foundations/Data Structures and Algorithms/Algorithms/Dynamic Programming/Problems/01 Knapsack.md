---
tags:
  - Algorithm
---
# Problem Statement: 0/1 Knapsack

You are given:

* A list of `n` items, each with:

  * a **weight** `w[i]`

  * a **value** `v[i]`

* A **knapsack** with a maximum capacity `W`.

**Goal**: Maximize the total value in the knapsack **without exceeding the weight limit**, and **you can’t take fractional items** (hence “0/1”).

---

# Dynamic Programming Approach

We build a **DP table** where:

* `dp[i][w]` \= max value that can be obtained with the first `i` items and a knapsack capacity `w`.

---

# Java Code Example

```java
public class Knapsack01 {

    // Returns the maximum value that can be put in a knapsack of capacity W
    public int knapsack(int[] weights, int[] values, int W) {
        int n = weights.length;

        int[][] dp = new int[n + 1][W + 1];

        // Build table dp[][] in bottom up manner
        for (int i = 1; i <= n; i++) {
            for (int w = 0; w <= W; w++) {
                if (weights[i - 1] <= w) {
                    // Include the item or exclude it
                    dp[i][w] = Math.max(
                        values[i - 1] + dp[i - 1][w - weights[i - 1]],
                        dp[i - 1][w]
                    );
                } else {
                    // Item can't be included
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }

        return dp[n][W]; // Max value with all items and full capacity
    }

    public static void main(String[] args) {
        Knapsack01 ks = new Knapsack01();
        int[] weights = {2, 3, 4, 5};
        int[] values = {3, 4, 5, 6};
        int capacity = 5;

        int maxValue = ks.knapsack(weights, values, capacity);
        System.out.println("Maximum value in knapsack: " + maxValue);
    }
}
```

---

# Output

```
Maximum value in knapsack: 7
```

Explanation: We can pick items with weights `2` and `3`, for a total value of `3 + 4 = 7`.

---

# Time and Space Complexity

* **Time**: O(n \* W)

* **Space**: O(n \* W)

You can optimize it to **O(W)** space using a 1D array if you iterate from high to low capacity (want that version too?).

---

# Summary

* Perfect when you're deciding **whether to include or exclude items**.

* Use when decisions have **overlapping subproblems**.

* Very common in **interviews and contests**.

