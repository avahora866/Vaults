---
tags:
  - Algorithm
  - MergeAlgorithm
  - FORMAT
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Selection sort is a **comparison-based**, **in-place**, and **unstable** sorting algorithm.
It repeatedly selects the minimum element from the unsorted part and places it at the beginning.

# Explanation
1. Loop through the array.
2. Find the minimum element from the unsorted portion.
3. Swap it with the first unsorted element.
4. Repeat for the remaining unsorted part.

Use it when:
* You want minimal swaps
* Simplicity > performance
* Working with small arrays or hardware with expensive write operations


## Time Complexity
| Case     | Time Complexity | Space Complexity |
| -------- | --------------- | ---------------- |
| Best     | O(n²)           | O(1)             |
| Average  | O(n²)           | O(1)             |
| Worst    | O(n²)           | O(1)             |
| Stable   | **No**          |                  |
| In-place | Yes             |                  |

## State Walkthrough
```
[64, 25, 12, 22, 11]
```

### Step 1:
```
[64, 25, 12, 22, 11]
```

`i = 0`, min = 11 → swap with 64

```
[11, 25, 12, 22, 64]
```

### Step 2:
```
[11, 25, 12, 22, 64]
```

`i = 1`
min = 12 → swap with 25

```
[11, 12, 25, 22, 64]
```

### Step 3:
```
[11, 12, 25, 22, 64]
```

`i = 2`, min = 22 → swap with 25

```
[11, 12, 22, 25, 64]
```

### Step 4:
```
[11, 12, 22, 25, 64]
```

`i = 3`, already sorted

```
[11, 12, 22, 25, 64]
```

# Example code
```java
import java.util.Arrays;

public class SelectionSort {

    public static void selectionSort(int[] arr) {
        int n = arr.length;

        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;

            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            if (minIndex != i) {
                int temp = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = temp;
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        System.out.println("Before sort: " + Arrays.toString(arr));
        selectionSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
