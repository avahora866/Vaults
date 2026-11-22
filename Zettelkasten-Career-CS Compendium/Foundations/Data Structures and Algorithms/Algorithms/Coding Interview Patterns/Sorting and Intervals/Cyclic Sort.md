---
tags:
  - Algorithm
  - SortingAlgorithm
  - ALGO_STUDY
---
Cyclic Sort is a sorting algorithm used on arrays of integer that are in a given range (usually 1 to n).

# Explanation

The algorithm works out the correct position to place each number due to the concept that a range of numbers has the following relationship with index. For each number:
$$
correctIndex = value - 1
$$

## Example
```
[1,2,3,4,5]
```
The correct position for value 1 is 1 - 1 = 0;

## Pre-requisites
- Works for numbers
- Numbers must be in a range
- Need to know the smallest value of the range.

## Time Complexity
Time complexity is O(n) with O(1) space.

## State Walkthrough
```
Range to sort: 3, 4, 6, 2, 1, 5 
```
### Step 1
Loop through array going taking away 1 from each value to find the correct position. If correct position is different from the current position swap the element
#### Loop 1a
Correct Postion = 3 - 1 = 2
Current Position = 0
Swap element to correct position
```
Array: 6, 4, 3, 2, 1, 5 
```
#### Loop 1b
Correct Postion = 6 - 1 = 5
Current Position = 0
Swap element to correct position
```
Array: 5, 4, 3, 2, 1, 6
```
#### Loop 1c
Correct Postion = 5 - 1 = 4
Current Position = 0
Swap element to correct position
```
Array: 1, 4, 3, 2, 5, 6
```
#### Loop 1d
Correct Postion = 1 - 1 = 0
Current Position = 0
Element at correct position move on to next index
```
Array: 1, 4, 3, 2, 5, 6
```
#### Loop 1e
Correct Postion = 4 - 1 = 3
Current Position = 1
Swap element to correct position
```
Array: 1, 2, 3, 4, 5, 6
```
#### Loop 1f
Correct Postion = 2 - 1 = 1
Current Position = 1
Element at correct position move on to next index
```
Array: 1, 2, 3, 4, 5, 6
```
#### Loop 1g
Correct Postion = 3 - 1 = 2
Current Position = 2
Element at correct position move on to next index
```
Array: 1, 2, 3, 4, 5, 6
```
#### Loop 1h
Correct Postion = 4 - 1 = 3
Current Position = 3
Element at correct position move on to next index
```
Array: 1, 2, 3, 4, 5, 6
```
#### Loop 1i
Correct Postion = 5 - 1 = 4
Current Position = 4
Element at correct position move on to next index
```
Array: 1, 2, 3, 4, 5, 6
```
#### Loop 1j
Correct Postion = 6 - 1 = 5
Current Position = 5
Element at correct position move on to next index
```
Array: 1, 2, 3, 4, 5, 6
```
### Step 2
Loop ends. Array is sorted
# Example code
```java
class CyclicSort {
    public void sort(int[] nums) {
        int i = 0;
        while (i < nums.length) {
            int correctPos = nums[i] - 1;
            if (nums[i] != nums[correctPos]) {
                swap(nums, i, correctPos);
            } else {
                i++;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```