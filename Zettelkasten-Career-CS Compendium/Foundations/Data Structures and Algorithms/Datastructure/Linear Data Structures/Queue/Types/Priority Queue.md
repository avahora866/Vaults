---
tags:
  - FLASHCARD
  - DataStructure
---
A `PriorityQueue` is a specialized queue that orders its elements based on their natural ordering (for Comparable elements) or a custom Comparator. Unlike a Queue, where elements follow FIFO (First-In-First-Out), a `PriorityQueue` always ensures that the smallest (or highest-priority) element is accessed first.

# Common Use Cases:

- Task scheduling: Used in job schedulers where high-priority tasks must be executed first.
- Graph algorithms: Dijkstra’s shortest path algorithm uses a priority queue to efficiently retrieve the minimum-cost node.
- [[Event-Driven Architecture|Event-driven]] simulations: Simulating real-world scenarios where events with different priority levels occur (e.g., hospital triage).

## Time Complexity

| Operation           | Time Complexity                    |
| ------------------- | ---------------------------------- |
| Insertion (offer()) | O(log n) (heapify-up)              |
| Removal (poll())    | O(log n) (heapify-down)            |
| Peek (peek())       | O(1) (access root)                 |
| Search              | O(n) (not optimized for searching) |

## Real-World Analogy

Think of a hospital emergency room:

- Patients arrive with different levels of urgency.
- Instead of treating them in arrival order, doctors prioritize critical cases first.
- When a new patient arrives, they are placed in the appropriate priority position.
- The highest priority (most critical) patient is always treated first.

This is exactly how a PriorityQueue operates: it always dequeues the highest-priority element first.
