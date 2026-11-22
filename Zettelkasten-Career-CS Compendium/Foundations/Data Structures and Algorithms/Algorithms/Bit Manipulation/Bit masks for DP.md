---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
---
Bit masking is a technique used in dynamic programming (DP) to efficiently represent and process subsets of a set. It is particularly useful when the problem involves choosing subsets, permutations, or combinations from a set of items, and the number of items is small enough that a bitmask representation can be used to represent all subsets.

# Concept of Bit Masking in DP:

In bitmask DP, we use an integer (often referred to as a "bitmask") to represent the state of a subset or configuration of a problem. Each bit in the integer corresponds to an element or choice in the set, and the value of the bit (either 0 or 1\) indicates whether that element is selected or not.

For a set of size `n`, a bitmask is a number with `n` bits. The bitmask value `0` represents the empty subset (i.e., no elements are chosen), and the bitmask value `2^n - 1` represents the subset where all elements are chosen.

# Basic Steps of Bit Mask DP:

1. **Representation**:  
    Use an integer to represent a subset. For example, for a set of `n` elements, use an integer with `n` bits to represent each subset.

   * If `i`\-th bit is `1`, the `i`\-th element is included in the subset.

   * If `i`\-th bit is `0`, the `i`\-th element is not included in the subset.

2. **Transitioning Between States**:  
    In a DP problem, you transition from one state to another by modifying the bitmask. For example, you might add an element to the subset (i.e., set a bit to `1`), or remove an element (i.e., set a bit to `0`).

3. **Iterating Over Subsets**:  
    To iterate over all subsets of `n` elements, you can loop through all possible bitmasks from `0` to `2^n - 1`.

# Example: Traveling Salesman Problem (TSP) Using Bit Masks

Let's look at a famous problem, the **Traveling Salesman Problem (TSP)**, where we want to find the shortest path that visits each city exactly once and returns to the origin city.

## Problem Description:

We are given `n` cities, and we need to find the minimum cost of visiting all cities exactly once and returning to the start.

## Approach Using Bit Masking:

We can represent the state of the problem using a bitmask:

* Each bit in the bitmask corresponds to a city. If the `i-th` bit is `1`, the `i-th` city has been visited; if it is `0`, the city has not been visited yet.

We define a DP state `dp[mask][i]`:

* `mask` represents the subset of cities visited so far (in the form of a bitmask).

* `i` represents the current city.

The DP transition would then look like this:

* From a state where we’ve visited cities corresponding to `mask`, and we are currently in city `i`, we can transition to a state where we visit city `j` (if `j` is not yet visited in the bitmask).

## Transition Formula:

* `dp[mask][i] = min(dp[mask ^ (1 << i)][j] + dist[j][i])`

  * `mask ^ (1 << i)` is the bitmask where city `i` is removed from the visited set (i.e., we're looking at the state before we visited city `i`).

  * `dist[j][i]` is the cost to travel from city `j` to city `i`.

## Algorithm:

```java
public class TSPBitmaskDP {
    static final int INF = Integer.MAX_VALUE;
    
    public static int tsp(int[][] dist) {
        int n = dist.length;
        int[][] dp = new int[1 << n][n];  // DP table
        // Initialize DP table with INF
        for (int[] row : dp) {
            Arrays.fill(row, INF);
        }
        
        // Starting point is the first city (index 0)
        dp[1][0] = 0;
        
        // Iterate through all subsets of cities
        for (int mask = 1; mask < (1 << n); mask++) {
            for (int u = 0; u < n; u++) {
                if ((mask & (1 << u)) == 0) continue;  // Skip if city u is not in the subset
                for (int v = 0; v < n; v++) {
                    if ((mask & (1 << v)) == 0) continue;  // Skip if city v is not in the subset
                    dp[mask][u] = Math.min(dp[mask][u], dp[mask ^ (1 << u)][v] + dist[v][u]);
                }
            }
        }
        
        // Return the minimum cost to visit all cities and return to the starting point
        int result = INF;
        for (int i = 1; i < n; i++) {
            result = Math.min(result, dp[(1 << n) - 1][i] + dist[i][0]);
        }
        return result;
    }

    public static void main(String[] args) {
        int[][] dist = {
            {0, 10, 15, 20},
            {10, 0, 35, 25},
            {15, 35, 0, 30},
            {20, 25, 30, 0}
        };

        int result = tsp(dist);
        System.out.println("Minimum cost of visiting all cities: " + result);
    }
}
```

# Explanation of the Code:

1. **DP Table**:

   * `dp[mask][i]` stores the minimum cost to visit all the cities specified by `mask` and end at city `i`.

   * `mask` is a bitmask where each bit represents whether a city has been visited.

2. **Initialization**:

   * We initialize the starting point, `dp[1][0] = 0`, which means we start at city 0 and only city 0 is visited.

3. **DP Transitions**:

   * For every possible subset of cities (`mask`), and for every city `u` that is in that subset, we try to move to every other city `v` in the subset and update the DP table based on the previous states.

4. **Final Result**:

   * The result is the minimum cost to visit all cities and return to the starting point, which is found by examining `dp[(1 << n) - 1][i] + dist[i][0]` for all cities `i`.

# Time Complexity:

* There are 2n2^n possible subsets of cities, and for each subset, there are `n` cities to check. For each city, we examine other cities, leading to a time complexity of O(n2⋅2n)O(n^2 \\cdot 2^n).

# Space Complexity:

* The DP table has dimensions 2n×n2^n \\times n, so the space complexity is O(n⋅2n)O(n \\cdot 2^n).

---

# Other Examples of Bitmask DP Problems:

1. **Subset Sum Problem**:

   * Find if a subset of the given set of numbers sums to a specific target. You can use a bitmask to represent all subsets and check if the sum of any subset equals the target.

2. **Partition Problem**:

   * Given a set of numbers, determine if it can be partitioned into two subsets with equal sum. This can be solved using bitmasking and dynamic programming.

3. **Job Scheduling**:

   * For problems like scheduling jobs with dependencies or resource constraints, bitmasking can represent the state of the system (which jobs are scheduled), and DP can track the optimal solution.

---

# Advantages of Bitmasking in DP:

* **Efficient Subset Representation**: Bitmasking provides an efficient way to represent and operate on subsets, which is very useful for DP problems involving combinations and subsets.

* **Compact Space Usage**: A bitmask is a compact way to store the state of a subset, which can save space compared to storing explicit sets or lists.

# Limitations:

* **Size of State Space**: Bitmask DP works best for problems where the number of elements (`n`) is small enough to allow 2n2^n subsets. For large values of `n`, the state space grows exponentially, and bitmasking may not be feasible.

# Summary:

Bitmasking in DP is a powerful technique for solving combinatorial problems involving subsets. By using integers to represent subsets, bitmask DP allows us to efficiently compute solutions to problems that would otherwise require much larger state spaces.

