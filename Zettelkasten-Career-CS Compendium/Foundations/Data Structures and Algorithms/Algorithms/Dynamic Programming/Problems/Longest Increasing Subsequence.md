---
tags:
  - Algorithm
---
Given an array of integers, find the **length of the longest subsequence** such that all elements of the subsequence are **in strictly increasing order**.

A subsequence is not necessarily contiguous — it just needs to preserve the **order**.

---

```
Input:  [10, 9, 2, 5, 3, 7, 101, 18]
LIS:    [2, 3, 7, 101]
Output: 4
```

---

# Dynamic Programming Approach (O(n²))

We define:

* `dp[i]` \= the length of the LIS **ending at index `i`**

We iterate `j` from `0` to `i-1`, and if `nums[i] > nums[j]`, we can extend that subsequence.

---

## Java Code (O(n²))

```java
public class LongestIncreasingSubsequence {

    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if (n == 0) return 0;

        int[] dp = new int[n];
        Arrays.fill(dp, 1); // each element is an LIS of length 1

        int maxLen = 1;

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }

        return maxLen;
    }

    public static void main(String[] args) {
        LongestIncreasingSubsequence lis = new LongestIncreasingSubsequence();
        int[] arr = {10, 9, 2, 5, 3, 7, 101, 18};
        System.out.println("Length of LIS: " + lis.lengthOfLIS(arr));
    }
}
```

---

## Time Complexity:

* **O(n²)**

---

# Optimized Approach with Binary Search (O(n log n))

You can reduce it using a clever trick:

* Maintain an array `tails[]`:

  * `tails[i]` stores the **smallest ending number of an increasing subsequence of length `i+1`**.

* Use **binary search** to find where to place or replace elements.

---

## Java Code (O(n log n))

```java
import java.util.*;

public class LISOptimized {

    public int lengthOfLIS(int[] nums) {
        List<Integer> tails = new ArrayList<>();

        for (int num : nums) {
            int idx = Collections.binarySearch(tails, num);

            if (idx < 0) idx = - (idx + 1); // insertion point

            if (idx == tails.size()) {
                tails.add(num);
            } else {
                tails.set(idx, num);
            }
        }

        return tails.size();
    }

    public static void main(String[] args) {
        LISOptimized lis = new LISOptimized();
        int[] nums = {10, 9, 2, 5, 3, 7, 101, 18};
        System.out.println("Length of LIS (Optimized): " + lis.lengthOfLIS(nums));
    }
}
```

---

## Time Complexity:

* **O(n log n)** using binary search

* **Space**: O(n) for `tails[]`

---

## Summary

| Method | Time | Space | Notes |
| ----- | ----- | ----- | ----- |
| DP (Nested loops) | O(n²) | O(n) | Simpler, good for small inputs |
| Binary Search Trick | O(n log n) | O(n) | Faster, but trickier to implement |

---

