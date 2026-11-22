---
tags:
  - Algorithm
  - ALGO_STUDY
---
**Activity Selection Problem**, also known as **Interval Scheduling**, using a greedy algorithm.

---

# Problem Statement

You're given `n` activities with their **start** and **end times**. Your goal is to select the **maximum number of non-overlapping activities**.

## Constraints:

* Only one activity can be performed at a time.

* An activity `A` with end time `e` cannot overlap with another activity `B` that starts before `e`.

---

# Example:

```
Activities (start, end):
(1, 3), (2, 4), (3, 5), (0, 6), (5, 9), (5, 7), (8, 9)

Optimal selection: (1, 3), (3, 5), (5, 7), (8, 9) → 4 activities
```

---

# Greedy Strategy:

Always select the activity that finishes **earliest**. Why?

Choosing the activity that ends first leaves the most room for the remaining activities.

---

# Steps:

1. **Sort** activities by **end time**.

2. Initialize `lastEnd = 0`.

3. Iterate over sorted activities:

   * If `activity.start >= lastEnd`, select it and update `lastEnd = activity.end`.

---

# Java Implementation

```java
import java.util.*;

class Activity {
    int start, end;

    Activity(int s, int e) {
        start = s;
        end = e;
    }
}

public class ActivitySelection {

    public static int maxActivities(List<Activity> activities) {
        // Sort by end time
        activities.sort(Comparator.comparingInt(a -> a.end));

        int count = 0;
        int lastEnd = 0;

        for (Activity act : activities) {
            if (act.start >= lastEnd) {
                count++;
                lastEnd = act.end;
            }
        }

        return count;
    }

    public static void main(String[] args) {
        List<Activity> activities = List.of(
            new Activity(1, 3),
            new Activity(2, 4),
            new Activity(3, 5),
            new Activity(0, 6),
            new Activity(5, 9),
            new Activity(5, 7),
            new Activity(8, 9)
        );

        System.out.println("Maximum non-overlapping activities: " + maxActivities(activities));
    }
}
```

---

# Time Complexity:

* Sorting: O(n log n)

* Selection: O(n)

* **Total:** O(n log n)

---

# Variants:

* Print selected activities

* Interval Partitioning (assign rooms instead of selecting)

* Weighted Interval Scheduling (use DP instead)

---

