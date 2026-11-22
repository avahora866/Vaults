---
tags:
  - Algorithm
  - ALGO_STUDY
---
The Top K elements algorithm is used to maintain K elements based on some ordering criteria. It uses a heap for various crtieria (min-heap for largest elements, max-heap for smallest elements)
# Explanation
Any problem that asks us to find the top/smallest/frequent ‘K’ elements among a given set falls under this pattern.

The best data structure to keep track of ‘K’ elements is [[Heap Binary Tree]]. This pattern will make use of the Heap to solve multiple problems dealing with ‘K’ elements at a time from a set of given elements. The pattern looks like this:

- Insert ‘K’ elements into the min-heap or max-heap based on the problem.  
- Iterate through the remaining numbers and if you find one that is larger than what you have in the heap, then remove that number and insert the larger one.

## Time Complexity
Time complexity is O(n log k) where k is the heap size.

## State Walkthrough
### Step 1: Understand the Input

You are given:

A list of numbers (e.g., `[4, 1, 2, 2, 3, 3, 3, 4, 4, 4]`)

A number K (e.g., K = 2), which represents how many top elements you need.

### Step 2: Count Frequency (If Finding Most Frequent Elements)

If the problem requires the K most frequent elements, count the occurrences of each element:

```
4 → 4 times
3 → 3 times
2 → 2 times
1 → 1 time
```

Store these frequencies in a data structure like a hash map (dictionary).

### Step 3: Use a Heap or Sorting Approach

There are two common ways to find the top K elements:

Approach 1: Min-Heap (Efficient)

1. Use a Min-Heap (Priority Queue) of size K to keep track of the K largest or most frequent elements.
2. Insert elements into the heap based on their frequency or value.
3. If the heap size exceeds K, remove the smallest element (ensuring only the top K remain).
4. At the end, extract all elements from the heap as the final answer.

Approach 2: Sorting (Simple but Slower)

1. Sort the elements by their frequency (or value).
2. Pick the top K elements from the sorted list.

### Step 4: Extract the Top K Elements

After processing, extract the elements remaining in the heap (if using a heap) or take the first K elements from the sorted list.

### Step 5: Return the Result

If we needed the K most frequent elements, the output for K = 2 in our example (`[4, 1, 2, 2, 3, 3, 3, 4, 4, 4]`) would be:

`[4, 3]`  (since 4 appeared 4 times, 3 appeared 3 times)

If we needed the K largest elements, the output for K = 2 from `[4, 1, 2, 2, 3, 3, 3, 4, 4, 4]` would be:

`[4, 3]`  (since 4 and 3 are the largest numbers)

### Summary of the Process

1. Count frequencies (if applicable).
2. Use a Min-Heap (for efficiency) or Sort the list (simpler but slower).
3. Extract the top K elements from the heap or sorted list.
4. Return the final list of top K elements.

This approach ensures we efficiently get the K largest or most frequent elements in O(N log K) (heap approach) or O(N log N) (sorting approach) time complexity.

# Example code
```java
class TopKElements {
    public int[] findTopK(int[] nums, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        for (int num : nums) {
            minHeap.offer(num);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = minHeap.poll();
        }
        
        return result;
    }
}
```