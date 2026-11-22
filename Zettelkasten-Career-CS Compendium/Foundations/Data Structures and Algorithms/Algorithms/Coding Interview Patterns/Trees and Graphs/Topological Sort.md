---
tags:
  - Algorithm
  - ALGO_STUDY
---
Topological Sorting creates an ordered sequence of vertices in a directed acyclic graph (DAG) such that for every edge (u,v), u comes before v in the sequence.

Uses either DFS or BFS with in-degree tracking. Used for dependency resolution and task scheduling.
# Explanation
Topological sort is an ordering of the vertices in a Directed Acyclic Graph (DAG) such that for every directed edge `u → v`, vertex `u` appears before `v` in the ordering.

## Prerequisite Concept: Directed Acyclic Graph (DAG)
- **Directed Graph**: A graph where edges have a direction, represented as arrows from one vertex to another.
- **Acyclic**: The graph must not contain any cycles, meaning no path leads back to a previously visited vertex.

A **DAG** ensures there are no cycles, making it suitable for topological sorting. Cycles in a graph make topological sorting impossible since it would create a contradiction in the order.

## Example Scenario: Course Scheduling
Consider a scenario where you need to take a set of courses but some have prerequisites:
- To take **Course 3**, you must complete **Course 1**.
- To take **Course 4**, you must complete **Courses 2 and 3**.

This can be represented as a DAG:

```
Course 1 → Course 3
Course 2 → Course 4
Course 3 → Course 4
```

### Approach to Topological Sort
One common algorithm for topological sorting is **Kahn’s Algorithm**. Here’s how it works:

1. **Calculate In-Degree**: Compute the in-degree (number of incoming edges) for each vertex *(Course 1 is a vertex as well as Course 3)*.
2. **Initialize Queue**: Add all vertices with an in-degree of `0` to a queue. (*Add all courses that dont have a dependency course to a queue*)
3. **Process the Queue**:
   - Remove a vertex from the queue and add it to the topological order.
	   - Queue: Course 1, Course 2, Course 3
	   - Remove Course 1 from the queue and add it to the topological order
		   - Queue: Course 2, Course 3
		   - Topological Order: Course 1
   - Reduce the in-degree of its neighbouring vertices by `1`.
	   - Course 3 in-degree (number of incoming edges) was 1 and is now 0
   - If any neighbour’s in-degree becomes `0`, add it to the queue.
	   - Queue: Course 3, Course 2, Course 3 (*Queues are FIFO (First In First Out)*)
1. **Check for Cycles**: If the topological order includes all vertices, the graph is a DAG. Otherwise, there’s a cycle.

## Time Complexity
Time complexity is O(V + E) where V is vertices and E is edges.

## State Walkthrough
#TODO

# Example code
```java
import java.util.*;

public class TopologicalSort {

    // Function to perform topological sort
    public static List<Integer> topologicalSort(int vertices, List<int[]> edges) {
        // Initialize in-degree array
        int[] inDegree = new int[vertices];
        Map<Integer, List<Integer>> adjacencyList = new HashMap<>();

        // Build the adjacency list and in-degree count
        for (int i = 0; i < vertices; i++) {
            adjacencyList.put(i, new ArrayList<>());
        }

        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            adjacencyList.get(u).add(v);
            inDegree[v]++;
        }

        // Collect all vertices with in-degree 0
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < vertices; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }

        List<Integer> topologicalOrder = new ArrayList<>();

        while (!queue.isEmpty()) {
            int currentNode = queue.poll();
            topologicalOrder.add(currentNode);

            // Reduce the in-degree of neighboring vertices
            for (int neighbor : adjacencyList.get(currentNode)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }

        // Check if topological ordering is possible
        if (topologicalOrder.size() == vertices) {
            return topologicalOrder;
        } else {
            throw new RuntimeException("The graph has a cycle and topological sorting is not possible.");
        }
    }

    public static void main(String[] args) {
        int vertices = 5;
        List<int[]> edges = Arrays.asList(
            new int[]{0, 2},
            new int[]{1, 2},
            new int[]{1, 3},
            new int[]{2, 4},
            new int[]{3, 4}
        );

        try {
            List<Integer> order = topologicalSort(vertices, edges);
            System.out.println("Topological Sort Order: " + order);
        } catch (RuntimeException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

## Explanation of the Code
- **`inDegree` array**: Tracks the number of incoming edges for each vertex.
- **`adjacencyList`**: Maps each vertex to its subsequent courses.
- **Queue**: Holds vertices with no prerequisites (in-degree 0).
- **`topologicalOrder`**: Stores the sorted order of courses.

**Step-by-step breakdown**:
1. Build the `adjacencyList` and initialize the `inDegree` array.
2. Add vertices with in-degree 0 to the queue.
3. Process each vertex, append it to the result, and adjust the in-degrees of its neighbors.
4. Ensure the graph is a DAG by confirming the result size matches the number of vertices.

