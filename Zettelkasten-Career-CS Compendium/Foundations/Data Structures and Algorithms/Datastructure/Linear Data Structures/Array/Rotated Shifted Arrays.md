---
tags:
  - DataStructure
---
A rotated sorted array is an array that was originally sorted in ascending order but has been rotated (shifted) at some pivot index. This means that a part of the sorted array has been moved to the other end while maintaining order within each part.

# Example of a Rotated Sorted Array

A sorted array:
`[1, 2, 3, 4, 5, 6, 7`]

After rotating at index 3, it becomes:
`[4, 5, 6, 7, 1, 2, 3]`

# Key Properties of a Rotated Sorted Array

1. Two sorted halves: One half of the array is always sorted.
2. Pivot element: The smallest element is the pivot (point of rotation).
3. [[Binary Searching Algorithms|Binary search]] can still be applied with modifications.
# Common Problems with Rotated Sorted Arrays

1. Finding an element in a rotated sorted array ([[Modified Binary Search|Modified Binary Search]]).
2. Finding the index of the minimum element (used to determine the rotation point).
3. Counting the number of rotations (index of the smallest element gives the rotation count).