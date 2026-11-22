---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
  - IMAGE_OCCLUSION_FLASHCARD
---
A* (A-star) is a **graph traversal and pathfinding algorithm**. It's widely for finding the **shortest path** between two nodes. It combines **Dijkstra’s Algorithm** and **Greedy Best-First Search** by considering both actual cost and heuristic cost.

# Explanation
A* searches by maintaining a **priority queue** (usually a min-heap) of paths to explore. Each path is evaluated using a **cost function**:

$$f
(n)=g(n)+h(n)f(n) = g(n) + h(n)
$$

- `f(n)` = total estimated cost of the cheapest solution through node `n`
- `g(n)` = cost from start node to `n` (actual cost)
- `h(n)` = heuristic estimated cost from `n` to goal

## How it works
1. **Initialize**:
    - Open set (priority queue) containing the start node
    - Closed set (set of visited nodes)
    - `g` scores: actual cost from start to node
    - `f` scores: estimated total cost through the node
2. **Loop**:
    - Pop the node `n` from open set with **lowest `f(n)`**
    - If `n` is the goal → return path
    - For each neighbor of `n`:
        - Skip if in closed set
        - Calculate tentative `g` score
        - If better than previous `g` score:
            - Update `g` and `f` scores
            - Set parent of neighbor to `n`
            - Add neighbor to open set
3. **Repeat** until goal found or open set is empty

## Heuristics
The heuristic function `h(n)` is **problem-specific** and critical for performance:

|Heuristic|When to Use|
|---|---|
|Manhattan Distance|Grid-based maps, no diagonal movement|
|Euclidean Distance|Movement in any direction|
|Zero|Reduces A* to Dijkstra's|

**Admissible**: never overestimates the cost  
**Consistent (Monotonic)**: h(n)≤c(n,n′)+h(n′)h(n) \leq c(n, n') + h(n')

## Time Complexity
| Feature          | Description                                             |
| ---------------- | ------------------------------------------------------- |
| Time Complexity  | Exponential in worst-case, depends on heuristic quality |
| Space Complexity | Also high (keeps all generated nodes in memory)         |
| Complete         | Yes (if graph is finite)                                |
| Optimal          | Yes, if `h(n)` is **admissible** (never overestimates)  |

## State Walkthrough
#TODO

# Example code
```java
import java.util.*;

class Node {
    int row;
    int col;
    // g: cost from start to this node
    double g; 
    // h: estimated cost from this node to target (heuristic)
    double h; 
    // f: total estimated cost (g + h)
    double f; 
    Node parent;

    public Node(int row, int col) {
        this.row = row;
        this.col = col;
        this.g = Double.MAX_VALUE;
        this.h = 0;
        this.f = Double.MAX_VALUE;
        this.parent = null;
    }

    // Overriding equals and hashCode is crucial for using Node in Sets/Maps
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Node node = (Node) o;
        return row == node.row && col == node.col;
    }

    @Override
    public int hashCode() {
        return Objects.hash(row, col);
    }

    @Override
    public String toString() {
        return "(" + row + ", " + col + ")";
    }
}

public class AStarPathfinder {

    // Grid dimensions
    private static final int ROWS = 5;
    private static final int COLS = 5;
    
    // Grid representation: 0 = free, 1 = obstacle
    // 
    private static final int[][] GRID = {
        {0, 0, 0, 0, 0},
        {0, 1, 1, 0, 0},
        {0, 1, 0, 0, 0},
        {0, 0, 0, 1, 0},
        {0, 0, 0, 0, 0}
    };

    /**
     * Calculates the Manhattan distance heuristic (h-cost)
     */
    private double calculateHeuristic(Node a, Node target) {
        // Manhattan distance: |x1 - x2| + |y1 - y2|
        return Math.abs(a.row - target.row) + Math.abs(a.col - target.col);
    }

    /**
     * Finds the shortest path using the A* algorithm.
     */
    public List<Node> findPath(Node start, Node target) {
        
        // PriorityQueue to store nodes to be examined (Open Set)
        // Sorts by f-cost (g + h)
        PriorityQueue<Node> openSet = new PriorityQueue<>(Comparator.comparingDouble(n -> n.f));
        
        // Set to store nodes that have already been examined (Closed Set)
        // Nodes in the closed set will not be checked again.
        Set<Node> closedSet = new HashSet<>();

        start.g = 0;
        start.h = calculateHeuristic(start, target);
        start.f = start.h;
        openSet.add(start);

        // Keep track of all created nodes to easily access them by coordinates
        // This is crucial for updating g-costs when a better path is found
        Node[][] allNodes = new Node[ROWS][COLS];
        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                allNodes[i][j] = new Node(i, j);
            }
        }
        allNodes[start.row][start.col] = start; // Ensure the start node is the correct instance

        while (!openSet.isEmpty()) {
            // Get the node with the lowest f-cost
            Node current = openSet.poll(); 
            
            // Re-check if this node is already processed with a better path
            if (closedSet.contains(current)) {
                continue;
            }
            
            closedSet.add(current);

            // Path found! Reconstruct and return the path.
            if (current.equals(target)) {
                return reconstructPath(current);
            }

            // Explore neighbors (up, down, left, right)
            int[][] directions = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}}; // (row_change, col_change)

            for (int[] dir : directions) {
                int newRow = current.row + dir[0];
                int newCol = current.col + dir[1];

                // 1. Check if the neighbor is within grid bounds
                if (newRow < 0 || newRow >= ROWS || newCol < 0 || newCol >= COLS) {
                    continue;
                }

                // 2. Check if the neighbor is an obstacle (1)
                if (GRID[newRow][newCol] == 1) {
                    continue;
                }
                
                // Get the actual Node instance from the allNodes array
                Node neighbor = allNodes[newRow][newCol];

                // 3. Check if the neighbor is already processed
                if (closedSet.contains(neighbor)) {
                    continue;
                }

                // Cost to move from current to neighbor is 1 (straight move)
                double tentativeGCost = current.g + 1;

                // If a shorter path to the neighbor is found
                if (tentativeGCost < neighbor.g) {
                    // Update the path (this is the key step)
                    neighbor.parent = current;
                    neighbor.g = tentativeGCost;
                    neighbor.h = calculateHeuristic(neighbor, target);
                    neighbor.f = neighbor.g + neighbor.h;

                    // If the neighbor is not in the open set, add it
                    if (!openSet.contains(neighbor)) {
                        openSet.add(neighbor);
                    } else {
                        // If it is already in the open set, the PriorityQueue
                        // may need to be refreshed if the f-cost changed.
                        // For simplicity here, we rely on the fact that
                        // a better path will still eventually be chosen. 
                        // For a robust solution, you might need to remove/re-add or use
                        // a different data structure or a custom update method.
                        // The simple addition of a new, better-costed node and relying on the
                        // closedSet check to discard the old one is often sufficient.
                        openSet.remove(neighbor);
                        openSet.add(neighbor);
                    }
                }
            }
        }

        // Open set is empty, and target was not reached
        return Collections.emptyList();
    }

    /**
     * Traces back through the parent pointers to build the final path.
     */
    private List<Node> reconstructPath(Node current) {
        List<Node> path = new ArrayList<>();
        Node temp = current;
        while (temp != null) {
            path.add(temp);
            temp = temp.parent;
        }
        // Path is built from target to start, so reverse it
        Collections.reverse(path); 
        return path;
    }

    public static void main(String[] args) {
        AStarPathfinder pathfinder = new AStarPathfinder();
        
        // Start at (0, 0)
        Node startNode = new Node(0, 0); 
        // Target at (4, 4)
        Node targetNode = new Node(4, 4); 

        System.out.println("Starting A* Pathfinding from " + startNode + " to " + targetNode);
        
        List<Node> path = pathfinder.findPath(startNode, targetNode);

        if (path.isEmpty()) {
            System.out.println("No path found to the target.");
        } else {
            System.out.println("Path found:");
            for (Node node : path) {
                System.out.print(node + " -> ");
            }
            System.out.println("END");
        }
    }
}
```