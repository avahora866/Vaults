---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
---
# Left Bound (Lower Bound)

This variation finds the first position where a target value could be inserted without violating the sort order — i.e. the smallest index `i` such that `arr[i] >= target`. It's commonly used in range queries, insertion problems, and frequency counting (e.g. number of elements less than a target). Standard in problems involving duplicates.

# Right Bound (Upper Bound)

This finds the last position where the target can be inserted without violating sort order — i.e. the largest index `i` such that `arr[i] <= target`. Useful for counting the number of elements less than or equal to a value. Often paired with left bound for computing element ranges.

# Search Insert Position

This returns the index where a value should be inserted to maintain sort order, regardless of whether the value exists. It’s a direct application of left bound. Useful in sorted array operations like lower bound in C++ STL, or binary insertion logic in problems like Leetcode 35.

# Binary Search on Answer

Instead of searching an array, this applies binary search to a range of **answers** where a predicate function determines feasibility. It’s used when the problem asks for the minimum/maximum value that satisfies a condition. Typical in optimization and allocation problems (e.g., minimum max load, smallest radius, Koko Eating Bananas).

# Rotated Sorted Array

This variant handles sorted arrays that have been rotated (e.g., `[4,5,6,7,0,1,2]`). It includes logic to detect the rotation point and decide which half to search. Common in interview problems like Leetcode 33 and 81, especially when combining rotation with duplicates.

# Peak Element Search

This variation finds a peak in an array where a peak is any element not smaller than its neighbors. It uses binary search to locate a peak in logarithmic time. Common in problems where you need to detect local maxima (e.g., Leetcode 162).

# K-th Element Search

Used to find the k-th smallest or largest element in two sorted arrays without full merge. It’s a more advanced binary search that operates across multiple arrays or constraints. Appears in problems like median of two sorted arrays.

# Floating Point Binary Search

Used on continuous domains (e.g. finding roots, distances, probabilities) with a precision threshold like `1e-6`. Instead of checking for equality, the condition is usually `abs(f(mid) - target) < epsilon`. Common in computational geometry, numerical methods, and floating-point optimization.

# Exponential Binary Search

This starts by exponentially expanding the bounds (1, 2, 4, 8...) until the target is bracketed, then performs binary search. Useful when the search space is unbounded or size unknown, like in infinite-sized arrays or stream-like data structures.

# Binary Search on Trees

A less common variant where binary search is applied on Binary Search Trees or Segment Trees. The structure maintains sorted order, and the logic mimics binary search. Applied in range search problems and persistent data structures.

Let me know if you want code examples or common pitfalls for each.
