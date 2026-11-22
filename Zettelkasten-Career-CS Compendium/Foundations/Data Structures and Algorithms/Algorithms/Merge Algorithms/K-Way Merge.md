---
tags:
  - Algorithm
  - MergeAlgorithm
  - ALGO_STUDY
---
The k-way merge algorithm **merges k sorted lists** into a single sorted list. Its primary application is in external sorting, where data is too large to fit into main memory. The data is sorted in chunks (or "runs") that fit in memory, written to disk, and then k-way merge combines these runs.

# Explanation
The algorithm's efficiency comes from using a [[Heap Binary Tree|min-heap]] (implemented as a [[PriorityQueue]] in Java) to track the smallest element across all k lists.
1. Create a min-heap that will store one element from each of the k lists.
2. Add the first element from each non-empty list to the heap. Each element in the heap must also store its original list index and its index within that list.
3. While the heap is not empty:
    - Remove the smallest element (the heap's root) and add it to the final result list.
    - Get the next element from the list that the removed element came from.
    - If a next element exists, add it to the heap.
    - If that list is now empty, do nothing.

This process repeats until all elements from all lists have been processed.

## Time Complexity
The time complexity is $O(N \log k)$, where $N$ is the total number of elements across all lists and $k$ is the number of lists.
- The heap **always holds at most $k$ elements**.
- Each insertion and extraction operation on the heap takes $O(\log k)$ time.
- You perform $N$ total insertions and extractions (one for each element).

This is much more efficient than the naive $O(N \cdot k)$ approach, where you would scan the front of all $k$ lists to find the minimum for each of the $N$ elements.

## State Walkthrough
Let's trace the algorithm with $k=3$ lists:
- List 0: [1, 5, 9]
- List 1: [2, 6]
- List 2: [3, 7, 10]

The heap state will show (value, listIndex, elementIndex).

|Step|Action                                                                         |minHeap State                  |result List              |
|----|-------------------------------------------------------------------------------|-------------------------------|-------------------------|
|0   |Initialize heap: Add first element from List 0, List 1, and List 2.            |(1, 0, 0), (2, 1, 0), (3, 2, 0)|[]                       |
|1   |Poll (1, 0, 0). Add 1 to result.   Add next from List 0 (index 1), which is 5. |(2, 1, 0), (3, 2, 0), (5, 0, 1)|[1]                      |
|2   |Poll (2, 1, 0). Add 2 to result.   Add next from List 1 (index 1), which is 6. |(3, 2, 0), (5, 0, 1), (6, 1, 1)|[1, 2]                   |
|3   |Poll (3, 2, 0). Add 3 to result.   Add next from List 2 (index 1), which is 7. |(5, 0, 1), (6, 1, 1), (7, 2, 1)|[1, 2, 3]                |
|4   |Poll (5, 0, 1). Add 5 to result.   Add next from List 0 (index 2), which is 9. |(6, 1, 1), (7, 2, 1), (9, 0, 2)|[1, 2, 3, 5]             |
|5   |Poll (6, 1, 1). Add 6 to result.   List 1 is now exhausted. Add nothing.       |(7, 2, 1), (9, 0, 2)           |[1, 2, 3, 5, 6]          |
|6   |Poll (7, 2, 1). Add 7 to result.   Add next from List 2 (index 2), which is 10.|(9, 0, 2), (10, 2, 2)          |[1, 2, 3, 5, 6, 7]       |
|7   |Poll (9, 0, 2). Add 9 to result.   List 0 is now exhausted. Add nothing.       |(10, 2, 2)                     |[1, 2, 3, 5, 6, 7, 9]    |
|8   |Poll (10, 2, 2). Add 10 to result.   List 2 is now exhausted. Add nothing.     |[]                             |[1, 2, 3, 5, 6, 7, 9, 10]|
|9   |Heap is empty. Terminate.                                                      |[]                             |[1, 2, 3, 5, 6, 7, 9, 10]|

# Example code
```java
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;

/**
 * Helper class to store the element's value, the index
 * of the list it came from, and its index within that list.
 */
class Node implements Comparable<Node> {
    int value;
    int listIndex;      // The index of the list in the outer list (0 to k-1)
    int elementIndex;   // The index of the element within its own list

    public Node(int value, int listIndex, int elementIndex) {
        this.value = value;
        this.listIndex = listIndex;
        this.elementIndex = elementIndex;
    }

    /**
     * Compare based on the element's value for the min-heap.
     */
    @Override
    public int compareTo(Node other) {
        return Integer.compare(this.value, other.value);
    }

    @Override
    public String toString() {
        // Helper for printing heap state
        return String.format("(v:%d, l:%d, e:%d)", value, listIndex, elementIndex);
    }
}

public class KWayMerge {

    /**
     * Merges k sorted lists into a single sorted list.
     *
     * @param lists A List containing k sorted Lists of Integers.
     * @return A single sorted List containing all elements.
     */
    public List<Integer> mergeKSortedLists(List<List<Integer>> lists) {
        if (lists == null || lists.isEmpty()) {
            return new ArrayList<>();
        }

        // A min-heap to store one node from each list
        // The size is k (the number of lists)
        PriorityQueue<Node> minHeap = new PriorityQueue<>();

        // 1. Initialize the heap with the first element from each list
        for (int i = 0; i < lists.size(); i++) {
            List<Integer> currentList = lists.get(i);
            if (currentList != null && !currentList.isEmpty()) {
                // Add the first element (index 0) from list i
                minHeap.add(new Node(currentList.get(0), i, 0));
            }
        }

        List<Integer> result = new ArrayList<>();

        // 2. Process the heap
        while (!minHeap.isEmpty()) {
            // 3a. Poll the smallest element
            Node minNode = minHeap.poll();
            result.add(minNode.value);

            // 3b. Get the next element from the same list
            int sourceListIndex = minNode.listIndex;
            int nextElementIndex = minNode.elementIndex + 1;
            
            List<Integer> sourceList = lists.get(sourceListIndex);

            // 3c. If that list has more elements, add the next one to the heap
            if (nextElementIndex < sourceList.size()) {
                int nextValue = sourceList.get(nextElementIndex);
                minHeap.add(new Node(nextValue, sourceListIndex, nextElementIndex));
            }
        }

        return result;
    }

    // Example Usage
    public static void main(String[] args) {
        List<Integer> list1 = List.of(1, 5, 9);
        List<Integer> list2 = List.of(2, 6);
        List<Integer> list3 = List.of(3, 7, 10);

        List<List<Integer>> allLists = List.of(list1, list2, list3);

        KWayMerge merger = new KWayMerge();
        List<Integer> mergedList = merger.mergeKSortedLists(allLists);

        System.out.println("Merged List: " + mergedList);
        // Output: Merged List: [1, 2, 3, 5, 6, 7, 9, 10]
    }
}
```
