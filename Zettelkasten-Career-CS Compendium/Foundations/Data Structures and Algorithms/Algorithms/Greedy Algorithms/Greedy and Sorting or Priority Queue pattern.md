---
tags:
  - Algorithm
  - ALGO_STUDY
---
**Greedy \+ Sorting / Priority Queue** pattern — a super common and effective technique in greedy algorithms.

---

It’s a greedy strategy that works like this:

* **Sort** or use a **priority queue (min-heap or max-heap)** to process elements in a **strategic order**.

* At each step, make the **best local choice** based on some condition.

* This order is usually determined by **value**, **deadline**, **end time**, **profit**, **ratio**, etc.

---

# Key Use Cases

| Problem Type | Technique |
| ----- | ----- |
| Activity selection | Sort by end time |
| Job scheduling with deadlines | Sort by deadline / use heap |
| Meeting rooms / intervals | Sort by start time \+ heap |
| Kth largest/smallest element | Heap (priority queue) |
| Huffman Encoding | Min-heap (by frequency) |
| Dijkstra’s algorithm | Min-heap (by distance) |

---

# Example 1: Meeting Rooms II (Leetcode 253\)

Given intervals, find the **minimum number of meeting rooms required**.

## Strategy:

* **Sort by start time**

* Use a **min-heap** to track current meeting end times

---

## Java Code:

```java
import java.util.*;

class Interval {
    int start, end;
    Interval(int s, int e) { start = s; end = e; }
}

public class MeetingRooms {

    public int minMeetingRooms(List<Interval> intervals) {
        if (intervals.isEmpty()) return 0;

        // Step 1: Sort by start time
        intervals.sort(Comparator.comparingInt(i -> i.start));

        // Step 2: Min-heap for end times
        PriorityQueue<Integer> heap = new PriorityQueue<>();

        for (Interval interval : intervals) {
            if (!heap.isEmpty() && heap.peek() <= interval.start) {
                heap.poll(); // Reuse room
            }
            heap.offer(interval.end); // Add new meeting
        }

        return heap.size(); // Number of rooms used
    }

    public static void main(String[] args) {
        List<Interval> intervals = List.of(
            new Interval(0, 30),
            new Interval(5, 10),
            new Interval(15, 20)
        );

        System.out.println("Min meeting rooms: " + new MeetingRooms().minMeetingRooms(intervals));
    }
}
```

---

# Example 2: Job Scheduling With Deadlines (Max Profit)

Each job has a deadline and profit. Do one job per unit time. Maximize total profit.

## Strategy:

* Sort jobs by **profit descending**

* Use **greedy** \+ **disjoint set** or **greedy** \+ **slot tracking**

Can also use priority queue if needed to store job options dynamically.

---

# Pattern Summary

| Feature | Sorting-Based Greedy | Heap-Based Greedy |
| ----- | ----- | ----- |
| Usage | One-time sorting | Dynamic ordering during iteration |
| Time Complexity | O(n log n) for sort | O(n log n) with priority queue |
| Data Structure | Arrays/List \+ sort | PriorityQueue |

---

# Pro Tip

When you have:

* Deadlines → sort by deadline

* Time slots → sort by end/start time

* Priority values → use max-heap or min-heap

* Continuous updates to priorities → prefer priority queue

---

