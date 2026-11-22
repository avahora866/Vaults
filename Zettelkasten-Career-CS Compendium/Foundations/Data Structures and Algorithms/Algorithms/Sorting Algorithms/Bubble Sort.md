---
tags:
  - Algorithm
  - MergeAlgorithm
  - FORMAT
  - ALGO_STUDY
---
Bubble sort is a simple comparison-based sorting algorithm. It works by **repeatedly swapping adjacent elements if they are in the wrong order**.

It's called "bubble" sort because smaller elements gradually **bubble up** to the beginning of the array, like bubbles rising in water.

# Explanation
Given an array of `n` elements:

1. Compare each pair of adjacent elements.
2. Swap them if they are out of order.
3. After one pass, the largest element is at the end.
4. Repeat for the remaining unsorted part of the array.

This process continues until the array is sorted.

## Time Complexity
| Case    | Time Complexity | Explanation                 |
| ------- | --------------- | --------------------------- |
| Best    | O(n)            | Already sorted + early stop |
| Average | O(n²)           | Nested loops                |
| Worst   | O(n²)           | Reverse sorted              |
| Space   | O(1)            | In-place sort               |


## State Walkthrough
Initial array:

```
[5, 1, 4, 2, 8]
```

## Pass 1:
* Compare 5 and 1 → swap → \[1, 5, 4, 2, 8]
* Compare 5 and 4 → swap → \[1, 4, 5, 2, 8]
* Compare 5 and 2 → swap → \[1, 4, 2, 5, 8]
* Compare 5 and 8 → no swap

Result after pass 1:

```
[1, 4, 2, 5, 8]
```

## Pass 2:
* Compare 1 and 4 → no swap
* Compare 4 and 2 → swap → \[1, 2, 4, 5, 8]
* Compare 4 and 5 → no swap

Result after pass 2:

```
[1, 2, 4, 5, 8]
```

## Pass 3:
* Compare 1 and 2 → no swap
* Compare 2 and 4 → no swap

No swaps → already sorted. Early termination.

# Example code
```java
import java.util.Arrays;

public class BubbleSort {

    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        boolean swapped;

        for (int pass = 0; pass < n - 1; pass++) {
            swapped = false;

            for (int i = 0; i < n - 1 - pass; i++) {
                if (arr[i] > arr[i + 1]) {
                    // Swap arr[i] and arr[i + 1]
                    int temp = arr[i];
                    arr[i] = arr[i + 1];
                    arr[i + 1] = temp;
                    swapped = true;
                }
            }

            // Optimization: stop if no swaps
            if (!swapped) break;
        }
    }

    public static void main(String[] args) {
        int[] arr = {5, 1, 4, 2, 8};
        System.out.println("Before sort: " + Arrays.toString(arr));
        bubbleSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
