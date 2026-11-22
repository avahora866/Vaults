---
tags:
  - Algorithm
  - MergeAlgorithm
  - FORMAT
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Merge sort is a **divide-and-conquer**, **comparison-based**, **stable**, and **not in-place** sorting algorithm.
It works by recursively splitting the array into halves, sorting each half, and then **merging** them back together.

# Explanation
1. Divide the array into two halves.
2. Recursively sort both halves.
3. Merge the sorted halves into one sorted array.

## Time Complexity
| Case     | Time Complexity | Space Complexity |
| -------- | --------------- | ---------------- |
| Best     | O(n log n)      | O(n)             |
| Average  | O(n log n)      | O(n)             |
| Worst    | O(n log n)      | O(n)             |
| Stable   | Yes             |                  |
| In-place | No              |                  |

## State Walkthrough
```
[38, 27, 43, 3, 9, 82, 10]
```

### Divide Phase (recursive calls)
```
Split into: [38, 27, 43] and [3, 9, 82, 10]
→ [38] [27, 43] → [27] [43]
→ [3, 9] [82, 10] → [3] [9], [82] [10]
```

### Merge Phase
* \[27] + \[43] → \[27, 43]
* \[38] + \[27, 43] → \[27, 38, 43]
* \[3] + \[9] → \[3, 9]
* \[82] + \[10] → \[10, 82]
* \[3, 9] + \[10, 82] → \[3, 9, 10, 82]
* \[27, 38, 43] + \[3, 9, 10, 82] → final sorted:

```
[3, 9, 10, 27, 38, 43, 82]
```

# Example code
```java
import java.util.Arrays;

public class MergeSort {

    public static void mergeSort(int[] arr) {
        if (arr.length < 2) return;
        mergeSortRecursive(arr, 0, arr.length - 1);
    }

    private static void mergeSortRecursive(int[] arr, int left, int right) {
        if (left >= right) return;

        int mid = left + (right - left) / 2;
        mergeSortRecursive(arr, left, mid);
        mergeSortRecursive(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }

    private static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] L = new int[n1];
        int[] R = new int[n2];

        for (int i = 0; i < n1; i++) L[i] = arr[left + i];
        for (int i = 0; i < n2; i++) R[i] = arr[mid + 1 + i];

        int i = 0, j = 0, k = left;

        // Merge the two halves
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) arr[k++] = L[i++];
            else arr[k++] = R[j++];
        }

        while (i < n1) arr[k++] = L[i++];
        while (j < n2) arr[k++] = R[j++];
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        System.out.println("Before sort: " + Arrays.toString(arr));
        mergeSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
