
# Pascal's Triangle – LeetCode 118 Solution

This repository contains a Java solution for **LeetCode Problem 118: Pascal's Triangle**.


## 🧩 Problem Statement

Given an integer `numRows`, generate the first `numRows` of **Pascal's Triangle**.
In Pascal's Triangle, each number is the sum of the two numbers directly above it.

**Example:**

```
Input: numRows = 5  
Output: [[1], [1,1], [1,2,1], [1,3,3,1], [1,4,6,4,1]]
```

---

## 💡 Solution

The solution is implemented in Java and iteratively builds Pascal's Triangle row by row.

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> triangle = new ArrayList<>();

        if (numRows == 0) return triangle;

        // First row
        triangle.add(new ArrayList<>());
        triangle.get(0).add(1);

        for (int i = 1; i < numRows; i++) {
            List<Integer> row = new ArrayList<>();
            List<Integer> prevRow = triangle.get(i - 1);

            row.add(1); // First element is always 1

            // Compute middle elements
            for (int j = 1; j < i; j++) {
                row.add(prevRow.get(j - 1) + prevRow.get(j));
            }

            row.add(1); // Last element is always 1

            triangle.add(row);
        }

        return triangle;
    }
}
```

---

## ⚙️ How It Works

1. **Initialization**

   * Create a `List<List<Integer>>` to store the triangle.
   * If `numRows == 0`, return an empty list.

2. **First Row**

   * Initialize the first row with `[1]`.

3. **Subsequent Rows**

   * For each row from index `1` to `numRows - 1`:

     * Start with `1`.
     * Compute middle elements by summing adjacent elements from the previous row.
     * End with `1`.
     * Add the row to the triangle.

4. **Output**

   * Return the full triangle.

---

## 🔍 Example Walkthrough

For `numRows = 3`:

```
Row 1: [1]  
Row 2: [1, 1]  
Row 3: [1, 1+1=2, 1] → [1, 2, 1]

Output: [[1], [1,1], [1,2,1]]
```

---

## 📈 Complexity Analysis

* **Time Complexity:** `O(numRows²)`

  * Each row `i` generates `i` elements → Total ≈ `numRows * (numRows + 1) / 2`.

* **Space Complexity:** `O(numRows²)`

  * The triangle stores all elements as a list of lists.

---

## 🚀 Usage

To use this solution:

1. Copy the Java code into your LeetCode submission or local Java environment.
2. Ensure you have Java installed (JDK 8 or later recommended).
3. Provide a non-negative integer input `numRows`.
4. Run the code to see the result.

---

## 💻 Running Locally

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        int numRows = 5;
        List<List<Integer>> result = solution.generate(numRows);
        System.out.println(result);
    }
}
``



