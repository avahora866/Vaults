---
tags:
  - FORMAT
  - Algorithm
---
**Unbounded Knapsack**, a variation of the 0/1 Knapsack problem — but with an important twist.

---

# What is the Unbounded Knapsack Problem?

You're still given:

* A list of `n` items.

  * Each item has a **weight `w[i]`** and **value `v[i]`**.

* A **maximum knapsack capacity** `W`.

**BUT** now you can use **unlimited quantities** of each item (you can take the same item multiple times).

---

# Key Difference from 0/1 Knapsack

| Aspect | 0/1 Knapsack | Unbounded Knapsack |
| ----- | ----- | ----- |
| Item usage | At most once | Unlimited |
| Inner loop direction | Right-to-left | Left-to-right |

---

# Dynamic Programming Approach

We use a 1D DP array where:

* `dp[w]` \= max value that can be achieved with capacity `w`.

We update the array in **ascending order** so that each item can be considered **multiple times**.

---

# Java Code Example

```java
public class UnboundedKnapsack {

    public int unboundedKnapsack(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[] dp = new int[W + 1];

        for (int w = 0; w <= W; w++) {
            for (int i = 0; i < n; i++) {
                if (weights[i] <= w) {
                    dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
                }
            }
        }

        return dp[W];
    }

    public static void main(String[] args) {
        UnboundedKnapsack uk = new UnboundedKnapsack();
        int[] weights = {2, 3, 4};
        int[] values = {5, 7, 9};
        int capacity = 10;

        int maxValue = uk.unboundedKnapsack(weights, values, capacity);
        System.out.println("Maximum value in knapsack: " + maxValue);
    }
}
```

---

# Output

```
Maximum value in knapsack: 25
```

Explanation: By taking item with weight `2` (value 5\) **five times**, we get `5 × 5 = 25`.

---

# Time and Space Complexity

* **Time**: O(n \* W)

* **Space**: O(W)

---

# Summary

* Use Unbounded Knapsack when you can **reuse items unlimited times**.

* Ideal for problems like:

  * Rod cutting

  * Coin change

  * Integer partitio
