
# 🧮 LeetCode 78 – Subsets

### 🧩 Problem:
Given an integer array `nums` of **distinct** integers, return **all possible subsets (the power set)**.  
The solution set **must not contain duplicate subsets**.

---

### 🔍 Example:

```text
Input: nums = [1,2,3]

Output: [
  [],          // empty subset
  [1],
  [2],
  [3],
  [1,2],
  [1,3],
  [2,3],
  [1,2,3]
]
````

---

### 💡 Approach: Backtracking (DFS-style)

We use **backtracking** to explore all combinations:

* For each index, decide whether to **include** or **exclude** the number.
* Build up a path (current subset) as you go.
* Backtrack by removing the last added number and continue exploring.

---

### ✅ Java Code

```java
import java.util.*;

public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        backtrack(0, nums, new ArrayList<>(), result);
        return result;
    }

    private void backtrack(int start, int[] nums, List<Integer> path, List<List<Integer>> result) {
        result.add(new ArrayList<>(path)); // Add current subset

        for (int i = start; i < nums.length; i++) {
            path.add(nums[i]);                   // choose
            backtrack(i + 1, nums, path, result); // explore
            path.remove(path.size() - 1);        // un-choose (backtrack)
        }
    }
}
```

---

### 🔁 Trace (nums = \[1, 2]):

```
[]
[1]
[1, 2]
[2]
```

---

### 🧠 Time & Space Complexity:

| Metric           | Complexity                           |
| ---------------- | ------------------------------------ |
| Time Complexity  | O(2^n) subsets                       |
| Space Complexity | O(n) recursion depth + O(2^n) output |

---

### ✅ Notes:

* This approach guarantees all subsets are covered with **no duplicates**.
* You can sort the array first if you want lexicographical order.

---

## ✨ Bonus: Iterative Solution (Bitmasking or Cascading)

If you're interested, this problem also has:

* **Bitmasking solution** (use binary representation)
* **Cascading approach** (build subsets iteratively)


