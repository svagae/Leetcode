# ♛ N-Queens – LeetCode 51

## 🧩 Problem Statement

> The N-Queens puzzle is the problem of placing `n` chess queens on an `n x n` chessboard such that no two queens attack each other.  
> 
> This means no two queens share the same **row**, **column**, or **diagonal**.  
>
> Return all distinct solutions to the n-queens puzzle. Each solution contains a board configuration represented as a list of strings.

---

## 📥 Example

### Input
```

n = 4

````

### Output
```json
[
  [".Q..",
   "...Q",
   "Q...",
   "..Q."],

  ["..Q.",
   "Q...",
   "...Q",
   ".Q.."]
]
````

Each string represents a row on the board:

* `'Q'` = Queen
* `'.'` = Empty

---

## ✅ Goal

Return **all valid configurations** of `n` queens on an `n x n` board such that no two queens attack each other.

---

## 🧠 Approaches

### 🔁 1. Brute Force (Backtracking with Full Checks)

**Idea**: Try placing a queen in each row, and for each placement, check the entire board to ensure no other queen can attack the current one.

* **Time Complexity**: O(n!)
* **Drawback**: Slow due to repeated diagonal & column checks.
* **Used in**: Early naive implementations or teaching examples.

---

### ✅ 2. Optimized Backtracking with HashSets (Recommended)

**Idea**:
Track which columns and diagonals are under attack using:

* `columns`: columns with a queen already
* `diag1`: `row - col` (↘ main diagonals)
* `diag2`: `row + col` (↙ anti-diagonals)

This lets us check for conflicts in **O(1)** time.

#### ✨ Benefits:

* Clean, readable
* Efficient due to pruning
* Elegant constraint handling

---

## ✍️ Java Code (Optimized Backtracking)

```java
public class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();

        // To track attacked columns and diagonals
        Set<Integer> columns = new HashSet<>();
        Set<Integer> diag1 = new HashSet<>(); // ↘ row - col
        Set<Integer> diag2 = new HashSet<>(); // ↙ row + col

        // Initialize empty board
        char[][] board = new char[n][n];
        for (char[] row : board) Arrays.fill(row, '.');

        backtrack(0, board, columns, diag1, diag2, result);
        return result;
    }

    private void backtrack(int row, char[][] board,
                           Set<Integer> columns,
                           Set<Integer> diag1,
                           Set<Integer> diag2,
                           List<List<String>> result) {

        int n = board.length;
        if (row == n) {
            result.add(constructBoard(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            if (columns.contains(col) || diag1.contains(row - col) || diag2.contains(row + col)) {
                continue; // skip threatened positions
            }

            // Place queen
            board[row][col] = 'Q';
            columns.add(col);
            diag1.add(row - col);
            diag2.add(row + col);

            backtrack(row + 1, board, columns, diag1, diag2, result);

            // Backtrack
            board[row][col] = '.';
            columns.remove(col);
            diag1.remove(row - col);
            diag2.remove(row + col);
        }
    }

    private List<String> constructBoard(char[][] board) {
        List<String> output = new ArrayList<>();
        for (char[] row : board) {
            output.add(new String(row));
        }
        return output;
    }
}
```

---

## 🧠 How Diagonals Work

For position `(row, col)`:

* Main diagonal ↘ = `row - col` (same value for all cells on the same ↘)
* Anti-diagonal ↙ = `row + col` (same value for all cells on the same ↙)

So we can **identify diagonals efficiently using math**, instead of checking each cell manually.

---

## 💡 Optimizations & Variations

### ➕ Bitmasking (Advanced)

Use integers as bitmasks to represent columns and diagonals instead of sets.
Faster for large `n` (e.g., `n = 12+`) and reduces memory.

* **Time**: Still exponential, but **faster in practice**
* **Space**: Reduced from O(n) to O(1) for sets

### ➕ N-Queens II (LeetCode 52)

Count the number of solutions instead of returning them.
Uses same logic — just increment a counter instead of storing boards.

---

## 🧪 Time & Space Complexity

| Operation        | Complexity                     |
| ---------------- | ------------------------------ |
| Time Complexity  | O(n!)                          |
| Space Complexity | O(n²) for board, O(n) for sets |

---

## 📌 Summary

| Feature         | Optimized Backtracking      |
| --------------- | --------------------------- |
| Conflict Checks | O(1) using sets             |
| Code Simplicity | ✅ Elegant                   |
| Memory          | Reasonable (no deep copies) |
| Scalability     | Good for `n <= 12`          |

---

## 📷 Visual Example (n = 4)

```
Solution 1:

. Q . .
. . . Q
Q . . .
. . Q .

Solution 2:

. . Q .
Q . . .
. . . Q
. Q . .
```

Each dot represents an empty cell, and `Q` marks the queen's position.

---

## 👑 Final Thoughts

The N-Queens problem is a **classic backtracking question** that teaches:

* How to model constraints
* Use of sets, recursion, and board generation
* Problem-solving with symmetry and math (diagonals)

Perfect for practicing DFS + pruning efficiently.

---


