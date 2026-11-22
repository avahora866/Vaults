---
tags:
  - Algorithm
  - MergeAlgorithm
  - SortingAlgorithm
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
Natural Merge focuses on **merging sorted subsequence's** rather than sorting from scratch.

# Explanation
Used in scenarios where the input might already have some sorted sequences:
- Identifies naturally sorted sequences
- Merges these sequences progressively
- Particularly efficient for partially sorted data

The key insights from this process are:
1. Natural runs reduce the total number of comparisons needed
2. Each pass reduces the number of runs by roughly half
3. The process continues until we have a single sorted run
4. If the data is already partially sorted, we need fewer passes

## Time Complexity
#TODO

## State Walkthrough
Unsorted array: 
`[23, 52, 14, 37, 41, 8, 95, 29, 17, 45]`

## Step 1: Identify Natural Runs (naturally sorted sequences)
The algorithm first scans the array to find sequences that are already in order:
```
[23, 52] | [14, 37, 41] | [8, 95] | [29] | [17, 45]

Current state:
- Run 1: [23, 52]
- Run 2: [14, 37, 41]
- Run 3: [8, 95]
- Run 4: [29]
- Run 5: [17, 45]
```

## Step 2: First Pass Merging (merge pairs of runs)
1. Merge Run 1 and Run 2:
   `[23, 52]` + `[14, 37, 41]`
   Comparison process:
   - 14 vs 23 → 14
   - 23 vs 37 → 23
   - 37 vs 52 → 37
   - 41 vs 52 → 41
   - 52 remains
   Result: `[14, 23, 37, 41, 52]`

2. Merge Run 3 and Run 4:
   `[8, 95]` + `[29]`
   Comparison process:
   - 8 vs 29 → 8
   - 29 vs 95 → 29
   - 95 remains
   Result: `[8, 29, 95]`

3. Run 5 remains unmapped: `[17, 45]`

After first pass, we have:
- Run A: `[14, 23, 37, 41, 52]`
- Run B: `[8, 29, 95]`
- Run C: `[17, 45]`

## Step 3: Second Pass Merging
4. Merge Run A and Run B:
   `[14, 23, 37, 41, 52]` + `[8, 29, 95]`
   Comparison process:
   - 8 vs 14 → 8
   - 14 vs 29 → 14
   - 23 vs 29 → 23
   - 29 vs 37 → 29
   - 37 vs 95 → 37
   - 41 vs 95 → 41
   - 52 vs 95 → 52
   - 95 remains
   Result: `[8, 14, 23, 29, 37, 41, 52, 95]`

5. Final merge with Run C:
  ` [8, 14, 23, 29, 37, 41, 52, 95]` + `[17, 45]`
   Final comparison process leads to:
   `[8, 14, 17, 23, 29, 37, 41, 45, 52, 95]`


# Example code
```java
public class NaturalMergeSort {
    public static void naturalMergeSort(int[] arr) {
        if (arr == null || arr.length < 2) return;
        
        boolean sorted = false;
        int[] temp = new int[arr.length];
        
        while (!sorted) {
            sorted = true;
            int left = 0; // Start of first run
            
            while (left < arr.length) {
                // Find the end of first sorted run
                int mid = findRunEnd(arr, left);
                if (mid == arr.length - 1) {
                    break; // We've reached the end of the array
                }
                
                // Find the end of second sorted run
                int right = findRunEnd(arr, mid + 1);
                
                // If we found two runs, merge them
                if (mid < right) {
                    merge(arr, temp, left, mid, right);
                    sorted = false; // We did a merge, so array might not be fully sorted
                }
                
                left = right + 1; // Move to start of next run
            }
        }
    }
    
    // Finds the end index of a naturally sorted run starting at 'start'
    private static int findRunEnd(int[] arr, int start) {
        int i = start;
        while (i < arr.length - 1 && arr[i] <= arr[i + 1]) {
            i++;
        }
        return i;
    }
    
    // Merges two sorted runs: arr[left..mid] and arr[mid+1..right]
    private static void merge(int[] arr, int[] temp, int left, int mid, int right) {
        // Copy both runs into temp array
        for (int i = left; i <= right; i++) {
            temp[i] = arr[i];
        }
        
        int i = left;     // Index for first run
        int j = mid + 1;  // Index for second run
        int k = left;     // Index for merged array
        
        // Merge runs back into arr
        while (i <= mid && j <= right) {
            if (temp[i] <= temp[j]) {
                arr[k++] = temp[i++];
            } else {
                arr[k++] = temp[j++];
            }
        }
        
        // Copy remaining elements if any
        while (i <= mid) {
            arr[k++] = temp[i++];
        }
        // No need to copy remaining elements from right run
        // as they are already in correct position
    }
    
    public static void main(String[] args) {
        int[] arr = {23, 52, 14, 37, 41, 8, 95, 29, 17, 45};
        
        System.out.println("Original array:");
        printArray(arr);
        
        naturalMergeSort(arr);
        
        System.out.println("\nSorted array:");
        printArray(arr);
    }
    
    private static void printArray(int[] arr) {
        for (int num : arr) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
}
```

