---
tags:
  - FORMAT
  - Algorithm
  - ALGO_STUDY
---
# Backtracking: Comprehensive Explanation

## What is Backtracking?

**Backtracking** is a **search algorithm** that incrementally builds candidates to solutions and abandons a candidate (backtracks) as soon as it determines that this candidate cannot lead to a valid solution.

It’s a form of **DFS (Depth-First Search)** with **constraint checking and pruning**. Unlike brute force, backtracking avoids unnecessary computation by discarding partial solutions early.

---

## How It Works

1. **Choose**: Select the next possible option (move, number, character, etc.)
2. **Constraint**: Check if this choice is valid (constraints satisfied)
3. **Explore**: Recur with this choice
4. **Un-choose (Backtrack)**: Undo the choice if it didn’t lead to a solution

This creates a **state tree**, and backtracking is used to traverse it intelligently.

---

## Structure of a Backtracking Algorithm

```java
void backtrack(State currentState) {
    if (solutionFound(currentState)) {
        recordSolution(currentState);
        return;
    }

    for (Option option : possibleOptions(currentState)) {
        if (isValid(currentState, option)) {
            apply(currentState, option);     // Choose
            backtrack(currentState);         // Explore
            undo(currentState, option);      // Un-choose
        }
    }
}
```

---

## Example Problem: N-Queens (Standard Backtracking Problem)

Place `N` queens on an `N x N` chessboard so that no two queens attack each other.

### Java Code

```java
import java.util.*;

public class NQueens {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> results = new ArrayList<>();
        char[][] board = new char[n][n];
        for (char[] row : board)
            Arrays.fill(row, '.');
        backtrack(0, board, results);
        return results;
    }

    private void backtrack(int row, char[][] board, List<List<String>> results) {
        if (row == board.length) {
            results.add(construct(board));
            return;
        }

        for (int col = 0; col < board.length; col++) {
            if (isValid(board, row, col)) {
                board[row][col] = 'Q';        // Choose
                backtrack(row + 1, board, results);  // Explore
                board[row][col] = '.';        // Un-choose (Backtrack)
            }
        }
    }

    private boolean isValid(char[][] board, int row, int col) {
        // Check vertical
        for (int i = 0; i < row; i++)
            if (board[i][col] == 'Q') return false;

        // Check diagonal \
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--)
            if (board[i][j] == 'Q') return false;

        // Check diagonal /
        for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++)
            if (board[i][j] == 'Q') return false;

        return true;
    }

    private List<String> construct(char[][] board) {
        List<String> result = new ArrayList<>();
        for (char[] row : board)
            result.add(new String(row));
        return result;
    }
}
```

---

## Time and Space Complexity

* **Time Complexity**: Worst case is `O(N!)` (factorial growth, since each row tries `N` options)
* **Space Complexity**: `O(N^2)` for board + recursion stack of depth `N`

---

## When to Use Backtracking

Use it when:

* The problem requires **combinations, permutations, or subsets**
* You can prune large portions of the search space using constraints
* You need **all solutions**, **first solution**, or **best solution** by some criteria

---

## Other Problems Solved by Backtracking

| Problem             | Notes                             |
| ------------------- | --------------------------------- |
| Subset Sum          | Search subsets that meet a target |
| Permutations        | Generate all permutations         |
| Sudoku Solver       | Fill grid using digit placement   |
| Word Search in Grid | Search path matching word         |
| Graph Coloring      | Assign colors without conflicts   |
| Combination Sum     | Combinations that sum to target   |

---

## Summary

* Backtracking = DFS + constraint checking + undo
* It’s used for solving **constraint satisfaction problems**
* Avoids brute force by pruning invalid paths early
* Core idea: **try, test, revert**

Let me know if you want examples for other problems like Sudoku, combinations, or permutations.
