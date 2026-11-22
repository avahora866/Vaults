---
tags:
  - Algorithm
  - FORMAT
  - ALGO_STUDY
  - MergeAlgorithm
  - IMAGE_OCCLUSION_FLASHCARD
---
Insertion sort is a **simple**, **comparison-based**, **in-place**, and **stable** sorting algorithm.
It builds the sorted array **one element at a time** by shifting elements as needed.

# Explanation
* Treat the first element as sorted.
* Pick the next element and insert it into the correct position in the sorted part by shifting elements to the right.

Use it when:
* Input size is small (`n` ≲ 30)
* Array is partially sorted
* You need a **stable**, in-place sort


## Time Complexity
| Case                   | Time Complexity | Space Complexity |
| ---------------------- | --------------- | ---------------- |
| Best (sorted input)    | O(n)            | O(1)             |
| Average                | O(n²)           | O(1)             |
| Worst (reversed input) | O(n²)           | O(1)             |
| Stable                 | Yes             |                  |
| In-place               | Yes             |                  |


## State Walkthrough
```
[5, 2, 4, 6, 1, 3]
```

### Step 1:
```
[5, 2, 4, 6, 1, 3]
```
i = 1, value = 2
5 is bigger than 2 = Shift 5
Insert 2 by shifting everything to the right
```
[2, 5, 4, 6, 1, 3]
```

### Step 2:
```
[2, 5, 4, 6, 1, 3]
```
i = 2, value = 4
5 is bigger than 4 = Shift 5
Insert 4 by shifting everything to the right
```
[2, 4, 5, 6, 1, 3]
```

### Step 3:
```
[2, 4, 5, 6, 1, 3]
```
i = 3, value = 6
6 is bigger than 5 = No shift needed
```
[2, 4, 5, 6, 1, 3]
```

### Step 4:
```
[2, 4, 5, 6, 1, 3]
```
i = 4, value = 1
Compare 6 > 1, 5 > 1, 4 > 1, 2 > 1 → shift all
Insert 1
```
[1, 2, 4, 5, 6, 3]
```

### Step 5:
```
[1, 2, 4, 5, 6, 3]
```
i = 5, value = 3
Compare 6 > 3, 5 > 3, 4 > 3 → shift
insert 3
```
[1, 2, 3, 4, 5, 6]
```

### Final Array:
```
[1, 2, 3, 4, 5, 6]
```

# Example code
```java
import java.util.Arrays;

public class InsertionSort {

    public static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int key = arr[i];
            int j = i - 1;

            // Shift larger elements to the right
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }

            // Insert key in correct position
            arr[j + 1] = key;
        }
    }

    public static void main(String[] args) {
        int[] arr = {5, 2, 4, 6, 1, 3};
        System.out.println("Before sort: " + Arrays.toString(arr));
        insertionSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
