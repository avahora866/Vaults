---
tags:
  - Algorithm
  - ALGO_STUDY
---
The Merge Intervals algorithm given an array of intervals that are sorted by start time merges any overlapping intervals.

# Explanation
## Interval
An interval is a the time between two numbers (integer, float, etc.)
Example:
```
[2, 5]
```
The first element must be lower then the second element
Can be represented by either an array with only 2 elements or an object with fields of start and end time

---
The Merge Interval is used for:
- Scheduling
- Resource Allocation
- Interval-based problems

## Time Complexity
Time complexity is O(n log n) due to initial sorting.

## State Walkthrough
1. Sort arrays by their start values
2. Merge any overlapping values
```
intervals = [[1, 3], [8, 10], [2, 6], [9, 12]]

Sorted intervals = [[1, 3], [2, 6], [8, 10], [9, 12]]
```

### Step 1: Sort the Intervals
Before merging, we sort the intervals based on their start times.

Sorted Intervals:
```
[[1, 3], [2, 6], [8, 10], [15, 18]]
```

(Since the input is already sorted, no changes here.)

### Step 2: Iterate and Merge
We maintain a merged list to store non-overlapping intervals.

#### Iteration 1: Processing `[1, 3]`
merged is empty → Add `[1, 3]`.

Merged state:

```
[[1, 3]]
```

#### Iteration 2: Processing `[2, 6]`
Compare `[2, 6]` with the last interval in merged: `[1, 3]`.

Since 2 <= 3, they overlap.

Merge them by taking the max end time: max(3, 6) = 6.

Replace `[1, 3]` with `[1, 6]`.

Merged state:

```
[[1, 6]]
```

#### Iteration 3: Processing `[8, 10]`
Compare `[8, 10]` with the last interval in merged: `[1, 6]`.

Since 8 > 6, no overlap → Add `[8, 10]` as a new interval.

Merged state:

```
[[1, 6], [8, 10]]
```

#### Iteration 4: Processing `[15, 18]`
Compare `[15, 18]` with the last interval in merged: `[8, 10]`.

Since 15 > 10, no overlap → Add `[15, 18]` as a new interval.

Merged state:

```
[[1, 6], [8, 10], [15, 18]]
```

### Final Output
The merged intervals are:

```
[[1, 6], [8, 10], [15, 18]]
```

This ensures all overlapping intervals are combined, and non-overlapping ones remain separate.

# Example code
```java
class Interval {
    int start;
    int end;
    Interval(int start, int end) {
        this.start = start;
        this.end = end;
    }
}

class MergeIntervals {
    public List<Interval> merge(List<Interval> intervals) {
        if (intervals == null || intervals.size() <= 1) {
            return intervals;
        }
        
        // Sort by start time
        Collections.sort(intervals, (a, b) -> a.start - b.start);
        
        List<Interval> merged = new ArrayList<>();
        Interval current = intervals.get(0);
        
        for (int i = 1; i < intervals.size(); i++) {
            if (current.end >= intervals.get(i).start) {
                current.end = Math.max(current.end, intervals.get(i).end);
            } else {
                merged.add(current);
                current = intervals.get(i);
            }
        }
        
        merged.add(current);
        return merged;
    }
}
```