## Problem Summary

You are given an array `nums` with `n` objects colored red (`0`), white (`1`), or blue (`2`). You need to sort them in-place so that objects of the same color are adjacent and appear in the order red, white, and blue.

---

## Initial Thought Process (Bubble Sort Inspired)

My first idea was inspired by Bubble Sort. Here's what I thought:

1. Loop through the array from the beginning.
2. Use two pointers: `i` and `i + 1`.
3. If `nums[i] > nums[i + 1]`, swap them.
4. If `nums[i] <= nums[i + 1]`, continue to the next pair.
5. At the end of the array, check if the array is sorted.
6. If it’s not sorted, recursively call the same method on the current array.

### Code for Initial Approach

```java
public class BubbleSortColors {
    public void sortColors(int[] nums) {
        if (isSorted(nums)) return;

        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i + 1]) {
                int temp = nums[i];
                nums[i] = nums[i + 1];
                nums[i + 1] = temp;
            }
        }
        sortColors(nums); // recursive call
    }

    private boolean isSorted(int[] nums) {
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > nums[i + 1]) return false;
        }
        return true;
    }
}
```

### Why This Approach Is Not Good

* **Time Complexity:** This is essentially a recursive version of Bubble Sort, which is **O(n^2)** in the worst case. Very inefficient for large arrays.
* **Space Complexity:** Recursion introduces additional call stack usage.
* **Unnecessary Sorting:** We are repeatedly comparing and swapping elements when a more targeted method exists.

This approach, although correct in outcome, is not suitable for interviews or production-level code due to its inefficiency.

---

## Optimal Approach: Dutch National Flag Algorithm

To solve this problem in **O(n)** time and **O(1)** space, we use the **Dutch National Flag Algorithm**, invented by Edsger Dijkstra. The core idea is to partition the array into three sections:

* All `0`s on the left.
* All `1`s in the middle.
* All `2`s on the right.

We achieve this using **three pointers**:

* `low`: the boundary for 0s.
* `mid`: the current index being inspected.
* `high`: the boundary for 2s.

### Code for Optimal Approach

```java
public class Solution {
    public void sortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.length - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                swap(nums, low, mid);
                low++;
                mid++;
            } else if (nums[mid] == 1) {
                mid++;
            } else { // nums[mid] == 2
                swap(nums, mid, high);
                high--;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### Step-by-Step Explanation

Let’s walk through the algorithm with an example:

#### Input:

```java
nums = [2, 0, 2, 1, 1, 0]
```

#### Initialization:

```
low = 0, mid = 0, high = 5
```

#### Iteration:

* `nums[mid] == 2` → swap with `high`, decrement `high`.
* `nums[mid] == 0` → swap with `low`, increment both `low` and `mid`.
* `nums[mid] == 1` → just increment `mid`.

Repeat this until `mid > high`. At the end, the array becomes:

```
[0, 0, 1, 1, 2, 2]
```

### Why This Is Better

| Feature           | Bubble Sort Approach  | Dutch National Flag |
| ----------------- | --------------------- | ------------------- |
| Time Complexity   | O(n^2)                | O(n)                |
| Space Complexity  | O(n) recursion stack  | O(1)                |
| In-place?         | Yes                   | Yes                 |
| Stable?           | No need for stability | Yes                 |
| Interview-worthy? | ❌ Too slow            | ✅ Optimal           |

---


