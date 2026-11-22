---
tags:
  - Algorithm
---
# Problem Statement

Given `n` points in a 2D plane, find the **pair of points with the smallest Euclidean distance** between them.

---

# Euclidean Distance Formula

For points `P1(x1, y1)` and `P2(x2, y2)`:

```
distance = √[(x2 - x1)² + (y2 - y1)²]
```

---

# Brute-Force Approach

Check all possible pairs and compute distances.

## Time Complexity:

* O(n²)

## Java Code:

```java
class Point {
    int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }
}

public class ClosestPairBruteForce {

    public static double closestPair(Point[] points) {
        double minDist = Double.MAX_VALUE;

        for (int i = 0; i < points.length; i++) {
            for (int j = i + 1; j < points.length; j++) {
                minDist = Math.min(minDist, distance(points[i], points[j]));
            }
        }

        return minDist;
    }

    private static double distance(Point a, Point b) {
        return Math.hypot(a.x - b.x, a.y - b.y);
    }

    public static void main(String[] args) {
        Point[] points = {
            new Point(2, 3), new Point(12, 30),
            new Point(40, 50), new Point(5, 1),
            new Point(12, 10), new Point(3, 4)
        };
        System.out.println("Closest distance: " + closestPair(points));
    }
}
```

---

# Optimized: Divide and Conquer Approach

## Time Complexity:

* O(n log n)

## Strategy:

1. **Sort points by X and Y coordinates**.
2. Recursively divide points into two halves.
3. Find closest pair in **left** and **right** halves.
4. Merge step: Check points across the dividing line (in a vertical strip).

## Why does it work?

Only **points within `d` units of the dividing line** (where `d` is the best distance so far) can possibly have a closer pair. Also, we only need to check at most **6-7 points ahead** in the strip due to geometry constraints.

---

# Java Implementation Sketch

Full implementation is long, but here's the outline:

```java
// 1. Sort points by x and y once
// 2. Recursively divide into two halves
// 3. After conquering, merge with strip analysis

// Helper: Only check up to 6-7 nearby points in Y-sorted strip
```

Would you like the full divide-and-conquer Java code with comments? It's a great one to step through\!

---

# Summary

| Method | Time Complexity | Notes |
| ----- | ----- | ----- |
| Brute Force | O(n²) | Easy but slow for large n |
| Divide & Conquer | O(n log n) | Efficient and elegant |

---

# Real-World Uses

* Geographic information systems (GIS)
* Clustering / spatial analysis
* Nearest neighbor problems
* Robotics and computer vision

