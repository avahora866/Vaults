---
tags:
  - FORMAT
  - Algorithm
---
**Heuristic and Cost-Based Tree Search** algorithms are used to find **optimal or near-optimal paths** in **state-space trees** (e.g. in AI, robotics, games, or route planning). Unlike pure BFS/DFS, they use **heuristics** and/or **path costs** to guide the search efficiently.

---

# Key Concepts

| Term                             | Definition                                                              |
| -------------------------------- | ----------------------------------------------------------------------- |
| **State Tree**                   | Nodes represent possible states; edges represent actions or transitions |
| **Cost** (`g(n)`)                | Accumulated path cost from the start node to node `n`                   |
| **Heuristic** (`h(n)`)           | Estimated cost from node `n` to the goal (based on domain knowledge)    |
| **Evaluation Function** (`f(n)`) | Guides the search. Varies per algorithm                                 |

---

# Algorithms

## A\ Search

* **`f(n) = g(n) + h(n)`**
* Uses actual cost + estimated future cost
* Guarantees shortest path if `h(n)` is **admissible** (never overestimates)
* Commonly used in maps, games, and puzzles

## Dijkstra’s Algorithm

* **`f(n) = g(n)`**
* Ignores heuristic; expands nodes by lowest cost so far
* Guaranteed optimal
* Equivalent to A\* with `h(n) = 0`

## Greedy Best-First Search

* **`f(n) = h(n)`**
* Ignores actual path cost
* Fast but **not optimal**
* Useful when speed is more important than accuracy

## Uniform Cost Search (UCS)

* Special case of Dijkstra
* Always expands the lowest cumulative cost node (`g(n)`)
* Optimal when costs are positive

## Iterative Deepening A\ (IDA\)

* A\* in depth-first iterations with `f(n)` cutoffs
* Less memory usage than A\*
* Useful for large trees (e.g., game search trees)

---

# Comparison Table

| Algorithm         | Uses `g(n)` | Uses `h(n)` | Optimal | Complete | Space |
| ----------------- | ----------- | ----------- | ------- | -------- | ----- |
| A\*               | Yes         | Yes         | Yes     | Yes      | High  |
| Dijkstra          | Yes         | No          | Yes     | Yes      | High  |
| Greedy Best-First | No          | Yes         | No      | No       | High  |
| UCS               | Yes         | No          | Yes     | Yes      | High  |
| IDA\*             | Yes         | Yes         | Yes     | Yes      | Low   |

---

# Heuristics

A **good heuristic** helps reduce the number of explored nodes:

* **Admissible**: Never overestimates (`h(n) ≤ actual remaining cost`)
* **Consistent (Monotonic)**: `h(n) ≤ cost(n, n') + h(n')`
* Example (pathfinding): use **Euclidean distance** or **Manhattan distance**

---

# Use Cases

* **Games (chess, Go)**: A\*, IDA\*, heuristic search
* **Route Planning**: A\*, Dijkstra
* **Robotics**: motion planning with heuristics
* **Puzzles** (8-puzzle, Rubik’s Cube): A\*, BFS with heuristics

---

# Summary

| Type             | Example Algorithms     | Strengths                | Weaknesses                      |
| ---------------- | ---------------------- | ------------------------ | ------------------------------- |
| Heuristic-Based  | A\*, Greedy Best-First | Faster if `h(n)` is good | Not guaranteed optimal (Greedy) |
| Cost-Based       | Dijkstra, UCS          | Optimal paths            | Slower, more memory             |
| Memory-Optimized | IDA\*                  | Low space use            | Slower, repeated work           |

Let me know your use case (e.g. games, search problems, robotics), and I can narrow down which algorithm and heuristics are most effective.
