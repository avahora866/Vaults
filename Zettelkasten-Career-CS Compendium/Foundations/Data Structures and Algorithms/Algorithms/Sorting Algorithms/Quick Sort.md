---
tags:
  - Algorithm
  - MergeAlgorithm
  - FORMAT
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Quick sort is a **divide-and-conquer**, **comparison-based**, **not stable**, and **in-place** sorting algorithm.
It partitions the array around a **pivot**, then recursively sorts the subarrays.

# Explanation
1. Choose a pivot.
2. Partition the array so that:
   * Elements < pivot go to the left.
   * Elements > pivot go to the right.
3. Recursively quick sort left and right partitions.

## Pivot Choice Options

| Strategy        | Description                                | Pros/Cons                             |
| --------------- | ------------------------------------------ | ------------------------------------- |
| Last element    | Simple, used in Lomuto partition           | Bad for sorted/reversed input (O(n²)) |
| First element   | Same pros/cons as last element             |                                       |
| Middle element  | Slightly better on some inputs             | Still O(n²) worst case                |
| Random element  | Prevents worst-case with high probability  | Requires RNG                          |
| Median-of-three | Median of (first, middle, last)            | Good balance of speed and safety      |
| True median     | Optimal partitioning (but O(n) to compute) | Rarely used due to cost               |


## Time Complexity
| Case     | Time Complexity | Space Complexity                                 |
| -------- | --------------- | ------------------------------------------------ |
| Best     | O(n log n)      | O(log n)                                         |
| Average  | O(n log n)      | O(log n)                                         |
| Worst    | O(n²)           | O(n) recursive stack (no tail call optimization) |
| Stable   | No              |                                                  |
| In-place | Yes             |                                                  |

## State Walkthrough
```
[10, 7, 8, 9, 1, 5]
```

Using **last element as pivot**:

### Step 1:
Array: `[10, 7, 8, 9, 1, 5]`
Pivot = 5
Partition: move elements < 5 left
After partition: `[1, 5, 8, 9, 10, 7]`
Pivot index = 1

### Step 2: 
Array: `[10, 7, 8, 9, 1, 5]`
Left subarray = `[1]` (sorted)

### Step 3: 
Array: `[10, 7, 8, 9, 1, 5]`
Right subarray = `[8, 9, 10, 7]`, pivot = 7
After partition: `[1, 5, 7, 9, 10, 8]`
Pivot index = 2

### Step 4: 
Array: `[1, 5, 7, 9, 10, 8]`
Right subarray = `[9, 10, 8]`, pivot = 8
After partition: `[1, 5, 7, 8, 10, 9]`
Pivot index = 3

### Step 5: 
Array: `[1, 5, 7, 8, 10, 9]`
Right subarray = `[10, 9]`, pivot = 9
After partition: `[1, 5, 7, 8, 9, 10]`

### Final array:
```
[1, 5, 7, 8, 9, 10]
```

# Example code
```java
import java.util.Arrays;

public class QuickSort {

    public static void quickSort(int[] arr) {
        quickSortRecursive(arr, 0, arr.length - 1);
    }

    private static void quickSortRecursive(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);
            quickSortRecursive(arr, low, pivotIndex - 1);
            quickSortRecursive(arr, pivotIndex + 1, high);
        }
    }

    // Lomuto partition scheme
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // Pivot choice
        int i = low;

        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                swap(arr, i, j);
                i++;
            }
        }

        swap(arr, i, high);
        return i;
    }

    private static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }

    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        System.out.println("Before sort: " + Arrays.toString(arr));
        quickSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```