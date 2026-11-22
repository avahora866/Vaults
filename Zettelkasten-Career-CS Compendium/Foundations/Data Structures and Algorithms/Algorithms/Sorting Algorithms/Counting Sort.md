---
tags:
  - Algorithm
  - FORMAT
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Counting sort is a **non-comparison** based sorting algorithm.
It works efficiently when:

* Input elements are **non-negative integers**
* The **maximum value (`k`)** is not significantly larger than the number of elements (`n`)

# Explanation
Instead of comparing elements, counting sort:

1. Counts the number of occurrences of each value.
2. Computes positions using prefix sums.
3. Places elements directly into the sorted output.

## When to Use

Use counting sort when:

* Input values are **non-negative integers**
* `k` (max value) is not much greater than `n`
* You need a **stable sort**

Avoid it when:

* Data range is large
* Values are floats, strings, or objects without integer keys

## Time Complexity
| Metric   | Complexity                          |
| -------- | ----------------------------------- |
| Time     | O(n + k)                            |
| Space    | O(n + k)                            |
| Stable   | Yes (maintains order of duplicates) |
| In-place | No (uses auxiliary arrays)          |

* `n`: number of elements in input
* `k`: range of input values (max value)


## State Walkthrough
Input:

```
[4, 2, 2, 8, 3, 3, 1]
```

### Step 1: Count occurrences

(max = 8)

```
count = [0, 1, 2, 2, 1, 0, 0, 0, 1]
           0  1  2  3  4  5  6  7  8
```

### Step 2: Prefix sums (cumulative count)
```
count = [0, 1, 3, 5, 6, 6, 6, 6, 7]
```

### Step 3: Build output (traverse input in reverse)

| i | arr\[i] | count\[arr\[i]] | output index | output                       |
| - | ------- | --------------- | ------------ | ---------------------------- |
| 6 | 1       | 1               | 0            | \[1, \_, \_, \_, \_, \_, \_] |
| 5 | 3       | 5               | 4            | \[1, \_, \_, \_, 3, \_, \_]  |
| 4 | 3       | 4               | 3            | \[1, \_, \_, 3, 3, \_, \_]   |
| 3 | 8       | 7               | 6            | \[1, \_, \_, 3, 3, \_, 8]    |
| 2 | 2       | 3               | 2            | \[1, \_, 2, 3, 3, \_, 8]     |
| 1 | 2       | 2               | 1            | \[1, 2, 2, 3, 3, \_, 8]      |
| 0 | 4       | 6               | 5            | \[1, 2, 2, 3, 3, 4, 8]       |

### Final output:

```
[1, 2, 2, 3, 3, 4, 8]
```

# Example code
```java
import java.util.Arrays;

public class CountingSort {

    public static void countingSort(int[] arr) {
        if (arr.length == 0) return;

        int max = Arrays.stream(arr).max().getAsInt();

        // Step 1: Count occurrences
        int[] count = new int[max + 1];
        for (int num : arr) {
            count[num]++;
        }

        // Step 2: Compute prefix sums
        for (int i = 1; i < count.length; i++) {
            count[i] += count[i - 1];
        }

        // Step 3: Build output array
        int[] output = new int[arr.length];
        for (int i = arr.length - 1; i >= 0; i--) {
            int num = arr[i];
            output[count[num] - 1] = num;
            count[num]--;
        }

        // Step 4: Copy back to original array
        System.arraycopy(output, 0, arr, 0, arr.length);
    }

    public static void main(String[] args) {
        int[] arr = {4, 2, 2, 8, 3, 3, 1};
        System.out.println("Before sort: " + Arrays.toString(arr));
        countingSort(arr);
        System.out.println("After sort:  " + Arrays.toString(arr));
    }
}
```
