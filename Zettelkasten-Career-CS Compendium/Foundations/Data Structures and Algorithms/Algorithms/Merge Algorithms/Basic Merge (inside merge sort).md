---
tags:
  - Algorithm
  - FORMAT
  - MergeAlgorithm
  - ALGO_STUDY
---
Basic **merge** refers to the merging step in **merge sort**, where two sorted arrays (or parts of an array) are combined into a single sorted array.

# Explanation
Basic Merge is an example of the **divide and conquer** strategy. 

The key idea is:
* Divide the input array into two halves.
* Recursively sort each half.
* Merge the sorted halves.

Used in **merge sort**, **external sorting**, and in problems that require merging sorted sequences.

## Time Complexity
Time compleexity is O(n) where n is the total number of elements in the two subarrays

## State Walkthrough
Initial Array: [2, 4, 6, 1, 3, 5]
Call: `merge(arr, 0, 2, 5)`
So we are merging:
- Left  = [2, 4, 6] (from index 0 to 2)
- Right = [1, 3, 5] (from index 3 to 5)

### Step 1: 
```
leftPointer = 0
rightPointer = 0
```

Compare left[0] and right[0]
1 is smaller
result[0] = 1
Increment rightPointer as we have merged it into result array

```
leftPointer = 0
rightPointer = 1
```
Array: [1]

### Step 2: 
```
leftPointer = 0
rightPointer = 1
```

Compare left[0] and right[1]
2 is smaller
result[1] = 2
Increment leftPointer as we have merged it into result array

```
leftPointer = 1
rightPointer = 1
```
Array: [1, 2]

### Step 3:
```
leftPointer = 1
rightPointer = 1
```

Compare left[1] and right[1]
3 is smaller
result[2] = 3
Increment leftPointer as we have merged it into result array

```
leftPointer = 2
rightPointer = 1
```
Array: [1, 2, 3]

### Step 4: 
```
leftPointer = 2
rightPointer = 1
```

Compare left[2] and right[1]
4 is smaller
result[3] = 4
Increment rightPointer as we have merged it into result array

```
leftPointer = 2
rightPointer = 2
```
Array: [1, 2, 3, 4]

### Step 5: 
```
leftPointer = 2
rightPointer = 2
```

Compare left[2] and right[2]
5 is smaller
result[4] = 5
Increment rightPointer as we have merged it into result array

```
leftPointer = 2
rightPointer = 3
```
Array: [1, 2, 3, 4, 5]

### Step 6: 
Remaining: left[2] = 6 → arr[5] = 6
```
leftPointer = 2
rightPointer = 3
```

Only one element left in left array 
result[5] = 6

Final array: [1, 2, 3, 4, 5, 6]

# Example code
```java
import java.util.Arrays;

public class MergeStep {

    public static void merge(int[] arr, int low, int mid, int high) {
        int n1 = mid - low + 1;
        int n2 = high - mid;

        // Create temporary arrays
        int[] left = new int[n1];
        int[] right = new int[n2];

        // Copy data
        for (int i = 0; i < n1; i++)
            left[i] = arr[low + i];
        for (int j = 0; j < n2; j++)
            right[j] = arr[mid + 1 + j];

        // Merge temp arrays back into arr[low..high]
        int i = 0, j = 0, k = low;
        while (i < n1 && j < n2) {
            if (left[i] <= right[j]) {
                arr[k++] = left[i++];
            } else {
                arr[k++] = right[j++];
            }
        }

        // Copy remaining elements of left[], if any
        while (i < n1) {
            arr[k++] = left[i++];
        }

        // Copy remaining elements of right[], if any
        while (j < n2) {
            arr[k++] = right[j++];
        }
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 1, 3, 5};
        System.out.println("Before merge: " + Arrays.toString(arr));
        merge(arr, 0, 2, 5);
        System.out.println("After merge: " + Arrays.toString(arr));
    }
}
```
