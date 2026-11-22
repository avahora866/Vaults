---
tags:
  - FLASHCARD
  - Algorithm
  - ALGO_STUDY
---
Dynamic programming (DP) is a problem-solving technique used in computer science and mathematics to solve problems by breaking them down into simpler subproblems. It is particularly useful for optimization problems, where the goal is to find the best solution from a set of possible solutions.

The key idea behind dynamic programming is to solve each subproblem just once and store its result in a table (or array) to avoid redundant computations. This process is called memoization (when storing the results of subproblems) or tabulation (when filling up a table iteratively).

# Key Concepts of Dynamic Programming:

1. Overlapping Subproblems: This means that the problem can be broken down into smaller, overlapping subproblems that are solved multiple times. DP avoids recalculating the same subproblem by storing its solution.
2. Optimal Substructure: This is when the optimal solution to a problem can be constructed efficiently from the optimal solutions to its subproblems. This property is essential for applying dynamic programming.

# Steps to Solve Problems Using Dynamic Programming:

1. Characterize the Structure of an Optimal Solution: Identify how the solution to the problem can be constructed from solutions to smaller subproblems.
2. Define the State: Define a state that represents a subproblem, typically in terms of the parameters that define the problem.
3. Recurrence Relation: Establish the relationship between the problem and its subproblems. This gives the formula to compute the optimal solution based on previous subproblem solutions.
4. Solve the Subproblems: Either recursively with memoization (top-down approach) or iteratively using tabulation (bottom-up approach).
5. Reconstruct the Solution (if needed): Once the DP table is built, use it to reconstruct the optimal solution.

# Example: Fibonacci Sequence

A classic example to explain dynamic programming is the Fibonacci sequence:

The Fibonacci sequence is defined as:


```
F(0) = 0

F(1) = 1

F(n) = F(n-1) + F(n-2) for n ≥ 2
```


Using a simple recursive approach, the Fibonacci sequence leads to many redundant calculations, for example:


```
F(2) = F(1) + F(0)

F(3) = F(2) + F(1)

And so on.
```

With dynamic programming, we can avoid this redundancy by storing the results of subproblems (the values of Fibonacci numbers we already computed).

## Top-down Approach (Memoization):
### Step by Step breakdown
#TODO 

```java
public class Fibonacci {
    private static int[] memo;

    public static int fib(int n) {
        if (n <= 1) return n;
        
        if (memo[n] != -1) return memo[n];
        
        memo[n] = fib(n - 1) + fib(n - 2);
        return memo[n];
    }

    public static void main(String[] args) {
        int n = 10;
        memo = new int[n + 1];
        Arrays.fill(memo, -1);
        System.out.println(fib(n));
    }
}
```

In this approach, we store the result of fib(n) in the memo array to avoid recalculating it multiple times.

## Bottom-up Approach (Tabulation):
### Step by Step breakdown
#TODO 

```java
public class Fibonacci {
    public static int fib(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }

    public static void main(String[] args) {
        int n = 10;
        System.out.println(fib(n));
    }
}
```

In this approach, we iteratively fill an array dp with the Fibonacci numbers from 0 to n.

## When to Use Dynamic Programming:

Optimization Problems: If the problem asks for the best or optimal solution (e.g., shortest path, longest subsequence, maximum profit).

Problems with Overlapping Subproblems and Optimal Substructure: If the problem can be broken down into overlapping subproblems whose optimal solutions combine to form the optimal solution to the original problem.


## Common Dynamic Programming Problems:

- Fibonacci Sequence
- Knapsack Problem
- Longest Common Subsequence (LCS)
- Matrix Chain Multiplication
- Shortest Path Problems (e.g., Dijkstra’s Algorithm)
- Coin Change Problem


Dynamic programming is a powerful technique to optimize recursive solutions and improve efficiency, especially for problems with overlapping subproblems.

