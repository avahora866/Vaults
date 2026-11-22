---
tags:
  - Algorithm
  - FORMAT
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Heap sort is a **comparison-based**, **in-place**, and **non-stable** sorting algorithm.
It uses a **binary heap** data structure to repeatedly extract the maximum (or minimum) element and build the sorted array.

# Explanation
* A **heap** is a binary tree with:
  * **Max-heap**: parent ≥ children → used for ascending sort
  * **Min-heap**: parent ≤ children → used for descending sort
* **Heapify**: procedure to maintain the heap property.
* **Build heap**: convert array into a valid heap.
* **Extract max**: swap root with last element, shrink heap, re-heapify.

## Time Complexity
| Case     | Time Complexity | Space Complexity |
| -------- | --------------- | ---------------- |
| Best     | O(n log n)      | O(1)             |
| Average  | O(n log n)      | O(1)             |
| Worst    | O(n log n)      | O(1)             |
| In-place | Yes             |                  |
| Stable   | No              |                  |

## State Walkthrough
```
[4, 10, 3, 5, 1]
```

### Step 1: Build max-heap
Heapify from last parent node (index 1):

* heapify at 1: 10 is already larger than children → no change

heapify at 0:

* left = 10, right = 3 → largest = 1 → swap 4 and 10

Result:

```
[10, 4, 3, 5, 1]
```

heapify at 1 again (after swap):

* left = 5 > 4 → swap 4 and 5
  → final max-heap:

```
[10, 5, 3, 4, 1]
```

### Step 2: Extract max repeatedly
**1st extract:**

* swap 10 and 1 → \[1, 5, 3, 4, 10]
* heapify at 0 → largest = 1 → swap 1 and 5 → \[5, 1, 3, 4, 10]
* heapify at 1 → swap 1 and 4 → \[5, 4, 3, 1, 10]

**2nd extract:**

* swap 5 and 1 → \[1, 4, 3, 5, 10]
* heapify → swap 1 and 4 → \[4, 1, 3, 5, 10]

**3rd extract:**

* swap 4 and 3 → \[3, 1, 4, 5, 10]
* heapify → 3 > 1 → no swap

**4th extract:**

* swap 3 and 1 → \[1, 3, 4, 5, 10]

Final sorted array:

```
[1, 3, 4, 5, 10]
```

# Example code
```java
import java.util.Arrays;

public class HeapSort {

    public static void heapSort(int[] arr) {
        int n = arr.length;

        // Step 1: Build max heap
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }

        // Step 2: Extract elements from heap one by one
        for (int i = n - 1; i >= 0; i--) {
            // Move current root to end
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            // Call heapify on reduced heap
            heapify(arr, i, 0);
        }
    }

    private static void heapify(int[] arr, int heapSize, int rootIndex) {
        int largest = rootIndex;
        int left = 2 * rootIndex + 1;
        int right = 2 * rootIndex + 2;

        if (left < heapSize && arr[left] > arr[largest]) {
            largest = left;
        }

        if (right < heapSize && arr[right] > arr[largest]) {
            largest = right;
        }

        if (largest != rootIndex) {
            int swap = arr[rootIndex];
            arr[rootIndex] = arr[largest];
            arr[largest] = swap;

            heapify(arr, heapSize, largest);
        }
    }

    public static void main(String[] args) {
        int[] arr = {4, 10, 3, 5, 1};
        System.out.println("Before sort: " + Arrays.toString(arr));
        heapSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
