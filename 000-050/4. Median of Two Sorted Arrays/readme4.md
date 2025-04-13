# Median of Two Sorted Arrays – Java Solution

This repository provides a clean and efficient Java implementation for solving **LeetCode #4: Median of Two Sorted Arrays** using **Binary Search** in **O(log(min(m, n)))** time complexity.

## 🧠 Problem Description

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the **median** of the two sorted arrays.

**You must solve the problem in O(log(min(m, n))) time.**

### 🔍 Example

```text
Input: nums1 = [1, 3], nums2 = [2]
Output: 2.0

Input: nums1 = [1, 2], nums2 = [3, 4]
Output: 2.5
```

---

## ✅ Approach: Binary Search on the Smaller Array

To achieve logarithmic time complexity, we perform **binary search on the smaller array**. We partition both arrays such that:

- Left half of both arrays contains half of the total elements (or one more for odd total).
- All elements on the left are less than or equal to all elements on the right.

### 📈 Time and Space Complexity

- **Time Complexity:** O(log(min(m, n)))  
- **Space Complexity:** O(1)

---

## 📄 Code Explanation

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
```

### ✅ Step 1: Ensure `nums1` is the smaller array

```java
        if (nums1.length > nums2.length) {
            int[] temp = nums1;
            nums1 = nums2;
            nums2 = temp;
        }
```

We swap arrays to guarantee binary search on the shorter one, minimizing time complexity.

### 🧮 Step 2: Set up variables

```java
        int m = nums1.length;
        int n = nums2.length;
        int low = 0;
        int high = m;
```

- `m` and `n`: lengths of the two arrays.
- `low` and `high`: binary search boundaries for `nums1`.

### 🔁 Step 3: Binary Search

```java
        while (low <= high) {
            int p1 = (low + high) / 2;
            int p2 = (m + n + 1) / 2 - p1;
```

- `p1` and `p2` are the partition indices for `nums1` and `nums2`.

```java
            int maxLeft1 = (p1 == 0) ? Integer.MIN_VALUE : nums1[p1 - 1];
            int maxLeft2 = (p2 == 0) ? Integer.MIN_VALUE : nums2[p2 - 1];
            int maxLeft = Math.max(maxLeft1, maxLeft2);

            int minRight1 = (p1 == m) ? Integer.MAX_VALUE : nums1[p1];
            int minRight2 = (p2 == n) ? Integer.MAX_VALUE : nums2[p2];
            int minRight = Math.min(minRight1, minRight2);
```

- Handle edge cases when partitions touch the edges of the arrays.
- Compute max of left parts and min of right parts.

```java
            if (maxLeft <= minRight) {
                if ((m + n) % 2 == 0) {
                    return (maxLeft + minRight) / 2.0;
                } else {
                    return maxLeft;
                }
            }
```

- If the partition is valid (`maxLeft ≤ minRight`), return the median:
  - Even total length: average of `maxLeft` and `minRight`
  - Odd total length: just `maxLeft`

```java
            else if (maxLeft1 > minRight2) {
                high = p1 - 1;
            } else {
                low = p1 + 1;
            }
```

- If partition is invalid, adjust binary search window accordingly.

```java
        }
        throw new IllegalArgumentException("Input arrays are not sorted.");
    }
}
```

- If no valid partition is found (should not happen with sorted inputs), throw an exception.

---

## 🧪 Sample Test Cases

```java
Solution s = new Solution();

System.out.println(s.findMedianSortedArrays(new int[]{1, 3}, new int[]{2})); // Output: 2.0
System.out.println(s.findMedianSortedArrays(new int[]{1, 2}, new int[]{3, 4})); // Output: 2.5
System.out.println(s.findMedianSortedArrays(new int[]{0, 0}, new int[]{0, 0})); // Output: 0.0
System.out.println(s.findMedianSortedArrays(new int[]{}, new int[]{1})); // Output: 1.0
```

---

## 🔍 Edge Cases Handled

- One array is empty
- All elements are equal
- Arrays of different lengths
- Even and odd total lengths

---

## 📌 Summary

This solution smartly reduces the search space using **binary search** on the smaller array and partitions both arrays to compute the median without fully merging them.
