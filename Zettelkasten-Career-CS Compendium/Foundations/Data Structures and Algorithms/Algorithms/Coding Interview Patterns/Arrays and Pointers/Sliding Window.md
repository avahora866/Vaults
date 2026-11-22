---
tags:
  - Algorithm
  - ALGO_STUDY
---
The Sliding Window algorithm uses two pointers to define a window which slides over the data incrementally, this maintains a subset of elements that will be processed.

# Explanation
There are two versions of the Sliding Window algorithm
- Fixed Sized Window: The window is the same size always
- Variables Sized Window: The window grows and shrinks through the program. It has no fixed size 

## Time Complexity
Time complexity is typically O(n) as each element is processed at most twice.

## State Walkthrough
### Fixed Sized Window
Find the maximum sum of any subarray of size `k = 3` in the array
```
array = [2, 1, 5, 1, 3, 2]
k = 3
```

#### Step 1: Find the first window value
```
[2, 1, 5]
maxSum = 8
windowSum = maxSum
```

#### Step 2: Slide the window to the right
We do not need to do 1 + 5 + 1. Instead use the previous window sum of 8 (calculated from 2 + 1 + 5). Take away 2 and add 1
```
[1, 5, 1]
windowSum = (8 - 2) + 1 = 7 
7 < 8 next loop 
```

#### Step 3: Slide again
We do not need to do 5 + 1 + 3. Instead use the previous window sum of 7 (calculated from 1 + 5 + 1). Take away 1 and add 3
```
[5, 1, 3]
windowSum = (7 - 1) + 3 = 9
9 > 8 
maxSum = windowSum
```

#### Step 4: Slide Again
We do not need to do 1 + 3 + 2. Instead use the previous window sum of 9 (calculated from 5 + 1 + 3). Take away 5 and add 2
```
[1, 3, 2]
windowSum = (9 - 5) + 2 = 6
6 < 9
```

Answer is 9

### Variable Sized Window
Find the smallest subarray with sum ≥ 7 in the array:
```
array = [2, 3, 1, 2, 4, 3]
targetSum >= 7
```

#### Step 1: Start expanding the window from the left
```
array = [2, 3, 1, 2, 4, 3]
```

```
window = [2] -> Sum = 2 (too small)
window = [2, 3] -> Sum = 5 (too small)
window = [2, 3, 1] -> Sum = 6 (too small)
window = [2, 3, 1, 2] -> Sum = 8 (valid, try to shrink to find a smaller version)
```

#### Step 2: Start shrinking from the left
```
array = [2, 3, 1, 2, 4, 3]
```

```
Remove 2 = [3, 1, 2] -> Sum = 6 (too small, stop shrinking)
Expand again to the right = [3, 1, 2, 4] -> Sum = 10 (valid, same size as previous)
Shrink from the left = [1, 2, 4] -> Sum = 7 (valid, smaller than previous)
```

#### Step 3: Continue Shrinking
```
array = [2, 3, 1, 2, 4, 3]
```

```
Remove 1 = [2, 4] -> Sum = 6 (too small, stop shrinking)
Expand to the right = [2, 4, 3] -> Sum = 9 (valid, shrink from the left)
Shrink from the left = [4, 3] -> Sum = 7 (valid, smalles window found)
```

# Example code
## Fixed Sized Window
Finds the maximum sum of any contiguous subarray of size k.

```java
    public static int findMaxSumSubarray(int[] arr, int k) {
        // Return 0 if the array is too small for the window.
        if (arr.length < k) {
            System.out.println("Invalid input: array length is smaller than window size k.");
            return 0;
        }

        int maxSum = 0;
        int windowSum = 0;

        // Calculate the sum of the initial window.
        for (int i = 0; i < k; i++) {
            windowSum += arr[i];
        }
        maxSum = windowSum;

        // Slide the window from the start to the end of the array.
        for (int windowEnd = k; windowEnd < arr.length; windowEnd++) {
            // Add the new element entering the window.
            // Subtract the element leaving the window.
            windowSum += arr[windowEnd] - arr[windowEnd - k];
            
            // Update the maximum sum if the current window's sum is greater.
            maxSum = Math.max(maxSum, windowSum);
        }

        return maxSum;
    }
```
## Variable Sized Window
This example finds the length of the smallest contiguous subarray whose sum is greater than or equal to a target S.

```java
    public static int findMinSubArray(int[] arr, int S) {
        int minLength = Integer.MAX_VALUE;
        int windowSum = 0;
        int windowStart = 0;

        // Expand the window by adding elements from the right.
        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            windowSum += arr[windowEnd];

            // Shrink the window from the left as long as the sum is >= S.
            while (windowSum >= S) {
                // Update the minimum length found so far.
                minLength = Math.min(minLength, windowEnd - windowStart + 1);
                
                // Subtract the element leaving the window and move the start pointer.
                windowSum -= arr[windowStart];
                windowStart++;
            }
        }

        // If minLength was not updated, no such subarray was found.
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
```

# Video walkthrough
https://www.youtube.com/watch?v=dOonV4byDEg

