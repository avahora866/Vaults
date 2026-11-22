---
tags:
  - Algorithm
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Modified Binary Search is a variation of [[Binary Searching Algorithms|binary search]] that handles
- [[Rotated Shifted Arrays]]
- finding the last or first occurrence
- finding the smallest or largest under a condition
by adapting the typical [[Binary Searching Algorithms]] conditions.

# Explanation
## How it works
To perform binary search on the rotated shifted array you will have to 
1. find middle
2. Which half is sorted
	1. compare the middle value with the start value. ( start bigger than middle = right half is sorted vice versa)
3. Is the target value in the sorted half. If so move pointers to the second half.
	1. Check if the target value is more than the middle value and less than the end value (or start and middle dependign on which half is the sorted half) and move pointers to the corresponding half
4. Repeat
```
Original sorted array: [1, 2, 3, 4, 5, 6, 7]
Rotated sorted array: [4, 5, 6, 7, 1, 2, 3]
Target element: 2

Start = 0
End = 6

Mid = 3 (value = 7)
The left half is the sorted half as the 7 is higher than the start value of 4. If it was lower than the sorted half would be on the right. 
Target of 2 is not in the left sorted half (start value is 4 which would be the lowest and is higher than 2)
So search the second half

Start = 4
End = 6
Mid = 5 (value = 2)
```

## Finding the first or last occurrence
1. Same as normal [[Binary Searching Algorithms|binary search]] until you find the target value.
2. Once found store the index and keep going one to the left or right (as [[Binary Searching Algorithms|binary search]] expects a sorted array any other occurrences will be to the left and right of the value you found) to find the first or last occurrence.
3. Keep going until the value you are on does not match to the target value
```

```
## Finding the smallest or largest under a condition
Usually something like search for target or less than target
```
Array = [1, 3, 5, 7, 9, 11]
Target = 6
Start = 0
End = 5

Mid = 2 (value = 5)
Mid is less than 6. Store it and move pointers to the right

Start = 3
End = 5
Mid = 4 (value = 9)
Mid is greater than 6. Move pointers to the left

Start = 3
End = 3
Mid = 3 (value = 7)
```

## Time Complexity
Time complexity remains O(log n) but with modified comparison logic.

## State Walkthrough
#TODO

# Example code
```java
class ModifiedBinarySearch {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Check which half is sorted
            if (nums[left] <= nums[mid]) {
                // Left half is sorted
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {
                // Right half is sorted
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
}
```