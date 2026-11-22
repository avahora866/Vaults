---
tags:
  - Algorithm
---
The **Fibonacci** sequence and its cousin, the **Climbing Stairs** problem. Both are foundational Dynamic Programming (DP) problems — simple in structure, but powerful for learning recursion, memoization, and tabulation.

---

# Fibonacci Sequence (Classic DP Example)

## Problem:

Find the `n`\-th Fibonacci number, where:

```
F(0) = 0
F(1) = 1
F(n) = F(n - 1) + F(n - 2)
```

---

## Recursive Solution (Inefficient)

```java
public int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2); // exponential time
}
```

❌ **Exponential Time** → Too slow for large `n`

---

## Memoization (Top-Down DP)

```java
public int fib(int n) {
    int[] memo = new int[n + 1];
    Arrays.fill(memo, -1);
    return fibMemo(n, memo);
}

private int fibMemo(int n, int[] memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}
```

✅ **Time**: O(n), **Space**: O(n)

---

## Tabulation (Bottom-Up DP)

```java
public int fib(int n) {
    if (n <= 1) return n;
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;

    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}
```

✅ **Time**: O(n), **Space**: O(n)

---

## Space Optimized (Constant Space)

```java
public int fib(int n) {
    if (n <= 1) return n;
    int a = 0, b = 1;
    for (int i = 2; i <= n; i++) {
        int temp = a + b;
        a = b;
        b = temp;
    }
    return b;
}
```

✅ **Time**: O(n), **Space**: O(1)

---

# Climbing Stairs (Same DP Structure as Fibonacci)

## Problem:

You’re climbing a staircase with `n` steps. You can take **1 or 2 steps** at a time. How many distinct ways can you climb to the top?

## Example:

```
Input: n = 4
Output: 5
Ways: (1,1,1,1), (1,1,2), (1,2,1), (2,1,1), (2,2)
```

---

## Observation:

Same recurrence as Fibonacci:

```
ways(n) = ways(n - 1) + ways(n - 2)
```

---

## Java Code (Optimized)

```java
public int climbStairs(int n) {
    if (n <= 2) return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) {
        int temp = a + b;
        a = b;
        b = temp;
    }
    return b;
}
```

✅ **Time**: O(n), **Space**: O(1)

---

# Summary

| Problem | Pattern | Recurrence | Notes |
| ----- | ----- | ----- | ----- |
| Fibonacci | DP Base Case \+ Recursion | F(n) \= F(n-1) \+ F(n-2) | Classic DP starter |
| Climbing Stairs | Same as Fibonacci | ways(n) \= ways(n-1) \+ ways(n-2) | Counts paths instead of values |

---

