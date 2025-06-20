# LeetCode 217 - Contains Duplicate

This solution uses a **modified insertion sort** to detect duplicates **while sorting** the array.

---

## 📝 Problem Statement

> Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

---

## 💡 Approach: Modified Insertion Sort with Duplicate Detection

### 🔍 Idea

Instead of sorting first then scanning for duplicates, this solution **sorts while checking for duplicates**.

It uses **insertion sort**, which builds a sorted section of the array one element at a time.

- For each element, we shift larger elements forward.
- While comparing with previous elements, we check:  
  👉 If a duplicate exists (`nums[j] == key`), return `true`.

If no duplicates are found after the sort, return `false`.

---

## 🔁 Code

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        for (int i = 1; i < nums.length; i++) {
            int key = nums[i];
            int j = i - 1;
            
            // Shift elements that are greater than key
            while (j >= 0 && nums[j] > key) {
                nums[j + 1] = nums[j];
                j--;
            }
            
            // Check for duplicate during shifting
            if (j >= 0 && nums[j] == key) {
                return true;
            }
            
            // Insert the element in its correct position
            nums[j + 1] = key;
        }
        return false;
    }
}
````

---

## 🔎 Step-by-Step Trace

### Input:

```java
nums = [3, 1, 4, 2, 3]
```

### Execution Steps:

| i | key | Array State       | Action                                     |
| - | --- | ----------------- | ------------------------------------------ |
| 1 | 1   | \[3, \_, 4, 2, 3] | 3 > 1 → shift → \[3, 3, 4, 2, 3]           |
|   |     |                   | insert 1 → \[1, 3, 4, 2, 3]                |
| 2 | 4   | \[1, 3, \_, 2, 3] | 3 < 4 → insert directly → \[1, 3, 4, 2, 3] |
| 3 | 2   | \[1, 3, 4, \_, 3] | 4 > 2 → shift → \[1, 3, 4, 4, 3]           |
|   |     |                   | 3 > 2 → shift → \[1, 3, 3, 4, 3]           |
|   |     |                   | insert 2 → \[1, 2, 3, 4, 3]                |
| 4 | 3   | \[1, 2, 3, 4, \_] | 4 > 3 → shift → \[1, 2, 3, 4, 4]           |
|   |     |                   | **nums\[2] == 3 → duplicate found!** ✅     |

---

## 🧠 Complexity Analysis

| Metric | Value              | Notes                                 |
| ------ | ------------------ | ------------------------------------- |
| Time   | `O(n²)` worst-case | Due to nested shifts (insertion sort) |
| Time   | `O(n)` best-case   | If already sorted                     |
| Space  | `O(1)`             | In-place sorting — no extra data used |

---

## ✅ Pros

* No additional memory usage
* Duplicate check is **embedded** in sorting
* Elegant for in-place solutions

---

## ❌ Cons

* Slow for large arrays (worst case `O(n²)`)
* Less readable compared to `HashSet` or built-in `sort()`

---

## 📊 Comparison Table

| Approach               | Time       | Space | Notes                         |
| ---------------------- | ---------- | ----- | ----------------------------- |
| Insertion Sort + Check | O(n²)      | O(1)  | Clever, but slow on large n   |
| HashSet                | O(n)       | O(n)  | Best general-purpose solution |
| Arrays.sort + scan     | O(n log n) | O(1)  | Clean and fast                |
| Brute-force loop       | O(n²)      | O(1)  | Simple, but worst performance |

---

## ✅ Recommendation

Use this approach if:

* You want a clean, in-place solution without extra memory.
* The input array is small or nearly sorted.
* You’re optimizing space usage.

For best performance on large datasets, use `HashSet` or `Arrays.sort()` instead.

---

