---
tags:
  - Algorithm
  - ALGO_STUDY
---
The Two heaps algorithm maintains two heaps (usually min and max) to track statistics of data stream like median. Max-heap stores smaller half, min-heap stores larger half.

Used for problems requiring partition of elements around a middle value. 

# Explanation
- Max Heap: A heap where the largest element is at the root.
- Min Heap: A heap where the smallest element is at the root.

Approach:
- Maintain a min heap of size k to store the top k largest elements in the array.
- Traverse through the array, adding each element to the heap.
- If the size of the heap exceeds k, remove the smallest element (from the root of the min heap).
- Once the array is fully processed, the root of the min heap will contain the kth largest element.

## Time Complexity
Time complexity is O(log n) per insertion with O(n) space.

## State Walkthrough
Let’s use the array 
```
[7, 10, 4, 3, 20, 15] 
```
and find the 3rd largest element (k = 3).

### Step 1: Initialize:

```
k = 3
```
Create an empty min heap.

### Iteration 1: Adding Element 7

The heap is empty. Insert 7.

Heap after inserting 7: 
```
[7]
```
(Min Heap with 1 element).

### Iteration 2: Adding Element 10

Heap has fewer than k elements, so insert 10.

Heap after inserting 10:
```
[7, 10]
```
(Min Heap with 2 elements).

Since the heap is still not full (size < k), no removal is needed.

### Iteration 3: Adding Element 4

Heap has fewer than k elements, so insert 4.

Heap after inserting 4: 
```
[4, 10, 7] 
```
(Min Heap with 3 elements).

The heap is now full with k elements.

### Iteration 4: Adding Element 3

The heap has k elements, but the new element (3) is smaller than the root of the heap (4), so we need to remove the smallest element and insert 3.

Remove the root (4) and insert 3.

Heap after insertion: 
```
[3, 10, 7] 
```
(Reorganizing the heap, the smallest element is removed, and the new heap is formed).

After reorganization, the heap becomes 
```
[3, 10, 7].
```

### Iteration 5: Adding Element 20

The heap has k elements, but the new element (20) is larger than the root (3), so we remove the smallest element (3) and insert 20.

Remove the root (3) and insert 20.

Heap after insertion:
```
[7, 10, 20] 
```
(Reorganizing the heap, the smallest element is removed, and the new heap is formed).

After reorganization, the heap becomes 
```
[7, 10, 20].
```

### Iteration 6: Adding Element 15

The heap has k elements, but the new element (15) is smaller than the root (7), so we leave the heap unchanged. We only consider elements larger than the root to maintain the top k largest elements.

Heap remains unchanged:
```
[7, 10, 20].
```

### Result:

After processing all the elements, the root of the min heap is the 3rd largest element in the array.

3rd largest element: 7.

### Summary of Heap States at Each Step

1. Initial heap: `[]`
2. After inserting 7: `[7]`
3. After inserting 10: `[7, 10]`
4. After inserting 4: `[4, 10, 7]`
5. After inserting 3: `[3, 10, 7]` (removed 4)
6. After inserting 20: `[7, 10, 20]` (removed 3)
7. After inserting 15: `[7, 10, 20]` (no change)

# Example code
```java
class MedianFinder {
    private PriorityQueue<Integer> maxHeap; // First half
    private PriorityQueue<Integer> minHeap; // Second half
    
    public MedianFinder() {
        maxHeap = new PriorityQueue<>((a, b) -> b - a);
        minHeap = new PriorityQueue<>();
    }
    
    public void addNum(int num) {
        maxHeap.offer(num);
        minHeap.offer(maxHeap.poll());
        
        if (maxHeap.size() < minHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
    
    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```