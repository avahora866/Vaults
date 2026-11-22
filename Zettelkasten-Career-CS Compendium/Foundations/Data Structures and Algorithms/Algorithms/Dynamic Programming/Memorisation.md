---
tags:
  - Algorithm
  - ALGO_STUDY
---
Memorization is an optimization technique used to improve the performance of recursive functions by storing the results of expensive function calls and reusing them when the same inputs occur again. It helps to avoid redundant calculations and reduces time complexity, especially in problems with overlapping subproblems like Fibonacci numbers, dynamic programming, and combinatorial problems.

# How It Works:

1. Store the result of a function call in a data structure (usually a `Map` or an array).
2. Before computing a result, check if it already exists in the data structure.
3. If found, return the stored result instead of recomputing.
4. If not found, compute the result, store it, and return it.

# Example: Fibonacci with Memorization (Java)

```java
import java.util.HashMap;
import java.util.Map;

public class MemoizationExample {
    private static Map<Integer, Long> memo = new HashMap<>();

    public static long fibonacci(int n) {
        if (n <= 1) return n; // Base case
        
        // Check if result is already computed
        if (memo.containsKey(n)) {
            return memo.get(n);
        }

        // Compute and store the result
        long result = fibonacci(n - 1) + fibonacci(n - 2);
        memo.put(n, result);
        
        return result;
    }

    public static void main(String[] args) {
        System.out.println(fibonacci(50)); // Optimized computation
    }
}
```

# Time Complexity:

- Without memorization: **O(2^n)** (Exponential)
- With memorization: **O(n)** (Linear)

Memorization is commonly used in dynamic programming problems to improve efficiency

> For reference see Climbing Stairs in Blind75Leetcode Project