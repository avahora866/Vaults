---
tags:
  - Algorithm
---
Bucket Sort is a distribution-based sorting algorithm that scatters elements into several groups called "buckets." You sort these buckets individually (typically using a different sorting algorithm like Insertion Sort) and then gather them in order to produce the final sorted list. It relies on the input data being uniformly distributed over a specific range to achieve linear time complexity.
# Explanation
The algorithm uses a "scatter-gather" approach. You divide the range of input values into $k$ equal-sized intervals (the buckets). You then iterate through the input array and place each element into the bucket corresponding to its value range.

After distribution, some buckets may contain multiple elements while others are empty. You sort each non-empty bucket individually. Finally, you visit the buckets in order and concatenate their elements to reconstruct the array.

**Requirements:**
- **Uniform Distribution:** The algorithm assumes input data is uniformly distributed across the range. If the data clusters efficiently into a few buckets, performance degrades to the speed of the inner sorting algorithm (often $O(n^2)$).
- **Known Range:** You must know the maximum and minimum values of the input to define the bucket intervals correctly.
- **Appropriate Data Types:** It is most commonly used for floating-point numbers in the range $[0, 1)$, though it adapts to integers.

## Time Complexity
|**Case**|**Time Complexity**|**Description**|
|---|---|---|
|**Best / Average**|$O(n + k)$|Occurs when elements are uniformly distributed. You spend $O(n)$ to scatter and $O(k)$ to process buckets.|
|**Worst**|$O(n^2)$|Occurs when all elements fall into a single bucket (clustering), forcing the inner sort (usually Insertion Sort) to handle all $n$ elements.|
|**Space Complexity**|$O(n + k)$|You need auxiliary space to store the $k$ buckets and the $n$ elements distributed within them.|

## State Walkthrough
Consider sorting the following array of floats with $n = 10$.
Input: [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]

### Step 1: Create Buckets:
You create 10 buckets (indexes 0-9). Each bucket covers a range of 0.1.

- Index 0: $[0.0, 0.1)$
- Index 1: $[0.1, 0.2)$ ...
- Index 9: $[0.9, 1.0)$

### Step 2: Scatter (Distribute Elements):
You place elements into buckets based on formula: floor(value * n).

- `0.78` $\rightarrow$ Bucket 7
- `0.17` $\rightarrow$ Bucket 1
- `0.39` $\rightarrow$ Bucket 3
- `0.26` $\rightarrow$ Bucket 2
- `0.72` $\rightarrow$ Bucket 7
- `0.94` $\rightarrow$ Bucket 9
- `0.21` $\rightarrow$ Bucket 2
- `0.12` $\rightarrow$ Bucket 1
- `0.23` $\rightarrow$ Bucket 2
- `0.68` $\rightarrow$ Bucket 6

**State of Buckets:**

- Bucket 1: `[0.17, 0.12]`
- Bucket 2: `[0.26, 0.21, 0.23]`
- Bucket 3: `[0.39]`
- Bucket 6: `[0.68]`
- Bucket 7: `[0.78, 0.72]`
- Bucket 9: `[0.94]`
- (Others are empty)

### **Step 4: Sort Individual Buckets:**
- Bucket 1: `[0.12, 0.17]`
- Bucket 2: `[0.21, 0.23, 0.26]`
- Bucket 7: `[0.72, 0.78]`

### Step 5: Gather (Concatenate):
Merge buckets in order (0 to 9).

Output: [0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]

# Example code
This implementation sorts floating point numbers in the range $[0, 1)$.
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class BucketSort {

    public static void bucketSort(float[] arr) {
        int n = arr.length;
        if (n <= 0) return;

        // 1. Create buckets
        // We use ArrayLists because we do not know how many elements will fall into each bucket
        @SuppressWarnings("unchecked")
        List<Float>[] buckets = new ArrayList[n];

        for (int i = 0; i < n; i++) {
            buckets[i] = new ArrayList<>();
        }

        // 2. Scatter: distribute elements into buckets
        for (float value : arr) {
            // Formula maps 0.0-1.0 to index 0-(n-1)
            int bucketIndex = (int) (value * n);
            
            // Safety check strictly for 1.0 if included in input
            if (bucketIndex >= n) {
                bucketIndex = n - 1;
            }
            
            buckets[bucketIndex].add(value);
        }

        // 3. Sort individual buckets and Gather
        int currentIndex = 0;
        for (int i = 0; i < n; i++) {
            // Only sort and merge if bucket has elements
            if (!buckets[i].isEmpty()) {
                Collections.sort(buckets[i]); // Uses Timsort (O(n log n) typically)
                
                // Overwrite original array
                for (float val : buckets[i]) {
                    arr[currentIndex++] = val;
                }
            }
        }
    }

    public static void main(String[] args) {
        float[] data = {0.78f, 0.17f, 0.39f, 0.26f, 0.72f, 0.94f, 0.21f, 0.12f, 0.23f, 0.68f};
        
        System.out.println("Original:");
        for (float f : data) System.out.print(f + " ");
        
        bucketSort(data);
        
        System.out.println("\nSorted:");
        for (float f : data) System.out.print(f + " ");
    }
}
```