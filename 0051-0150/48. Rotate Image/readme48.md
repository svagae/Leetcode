### ✅ Correct Approach (In-Place)

To rotate the matrix **in-place**, you can follow **two steps**:

1. **Transpose the matrix** (swap `matrix[i][j]` with `matrix[j][i]`)
2. **Reverse each row**

---

### ✅ Final Java Code (In-place version):

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose the matrix
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // Swap elements at (i, j) and (j, i)
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < n; i++) {
            reverseRow(matrix[i]);
        }
    }

    private void reverseRow(int[] row) {
        int left = 0, right = row.length - 1;
        while (left < right) {
            int temp = row[left];
            row[left] = row[right];
            row[right] = temp;
            left++;
            right--;
        }
    }
}
```

---

### 🧾 README Explanation (for GitHub or docs)

```markdown
## 🌀 Rotate Image – LeetCode 48

### Problem:
Given an `n x n` 2D matrix representing an image, rotate the image by 90 degrees (clockwise). The rotation must be done **in-place**, meaning no additional matrix should be used.

---

### 🔄 Approach:

To rotate the matrix 90° clockwise in-place:

1. **Transpose the matrix** – Convert rows to columns by swapping `matrix[i][j]` with `matrix[j][i]`.
2. **Reverse each row** – This aligns the matrix to complete the clockwise rotation.

---

### ✅ Example:

Input:
```

\[
\[1,2,3],
\[4,5,6],
\[7,8,9]
]

```

After transpose:
```

\[
\[1,4,7],
\[2,5,8],
\[3,6,9]
]

```

After reversing each row:
```

\[
\[7,4,1],
\[8,5,2],
\[9,6,3]
]

```

---

### ✅ Time Complexity:
- O(n²) for both transpose and row reversal.

### ✅ Space Complexity:
- O(1) — All operations are done in-place.

---

### 📌 Notes:
- Avoid allocating new matrices like `int[][] result = ...` as this breaks the in-place requirement.
```

---

