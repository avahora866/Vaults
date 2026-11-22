---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Binary Search is an efficient algorithm for finding an element in a **sorted array**. It repeatedly divides the search interval in half and eliminates half of the remaining elements from consideration.

# Explanation
1. Initialize two pointers: `low` at the start, `high` at the end of the array.
2. Compute the middle index: `mid = low + (high - low) / 2`.
3. Compare `array[mid]` to the target:
    - If equal, return `mid`.
    - If target < `array[mid]`, narrow search to the left: `high = mid - 1`.
    - If target > `array[mid]`, narrow search to the right: `low = mid + 1`.
4. Repeat until `low > high`.

## Conditions for Binary Search
- The data must be **sorted** (ascending or descending).
- You must be able to access elements by index (i.e. array or random-access 
structure).

## Time Complexity
|Case|Time Complexity|
|---|---|
|Best|O(1)|
|Avg|O(log n)|
|Worst|O(log n)|

## State Walkthrough
Let’s walk through searching for `7` in the array `{1, 3, 5, 7, 9, 11, 13}`:

|Iteration|low|high|mid|array[mid]|Action|
|---|---|---|---|---|---|
|1|0|6|3|7|Match found, return 3|

Searching for `6` instead:

|Iteration|low|high|mid|array[mid]|Action|
|---|---|---|---|---|---|
|1|0|6|3|7|6 < 7 → high = 2|
|2|0|2|1|3|6 > 3 → low = 2|
|3|2|2|2|5|6 > 5 → low = 3 > high → not found|

### Edge Cases

- Empty array → return `-1`
- One-element array → works as expected
- Duplicates → returns any one matching index


# Example code
## Iterative
```java
public class BinarySearchIterative {
    public static int binarySearch(int[] arr, int target) {
        int low = 0, high = arr.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target)
                return mid;
            else if (arr[mid] < target)
                low = mid + 1;
            else
                high = mid - 1;
        }

        return -1; // Not found
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 8, 10, 12, 14};
        System.out.println(binarySearch(arr, 10)); // Output: 4
    }
}
```

## Recursive
```java
public class BinarySearchRecursive {
    public static int binarySearch(int[] arr, int target, int low, int high) {
        if (low > high) return -1;

        int mid = low + (high - low) / 2;

        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            return binarySearch(arr, target, mid + 1, high);
        else
            return binarySearch(arr, target, low, mid - 1);
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 8, 10, 12, 14};
        System.out.println(binarySearch(arr, 10, 0, arr.length - 1)); // Output: 4
    }
}
```
