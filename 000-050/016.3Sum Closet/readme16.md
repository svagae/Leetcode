# 🧩 LeetCode 16: 3Sum Closest – Java Solution

## 🚀 Overview

This repository provides a Java solution to the **3Sum Closest** problem from LeetCode.

> **Problem Statement**:  
Given an integer array `nums` and an integer `target`, return the sum of the three integers in `nums` such that the sum is closest to `target`.

---

## 🧠 Approach

The solution uses the **two-pointer technique** after **sorting** the array.

### ✔️ Key Steps:
1. **Sort** the input array.
2. Iterate through the array and fix one number.
3. Use two pointers (`left` and `right`) to find the other two numbers.
4. Track and update the **closest sum** based on how near it is to the target.
5. Adjust pointers accordingly to get closer to the target sum.

---

## 💻 Java Code

```java
import java.util.Arrays;

class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums); // Sort the array to use two pointers
        int closestSum = nums[0] + nums[1] + nums[2]; // Initial closest sum

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int currentSum = nums[i] + nums[left] + nums[right];

                if (Math.abs(currentSum - target) < Math.abs(closestSum - target)) {
                    closestSum = currentSum; // Update if closer
                }

                if (currentSum < target) {
                    left++;  // Need a larger sum
                } else {
                    right--; // Need a smaller sum
                }
            }
        }

        return closestSum;
    }
}
```

---

## 🧪 Example

### Input
```java
nums = [-1, 2, 1, -4]
target = 1
```

### Output
```
2
```

> Because `-1 + 1 + 2 = 2`, which is the closest sum to `1`.

---

## ⏱️ Time & Space Complexity

| Type            | Complexity |
|-----------------|------------|
| Time Complexity | O(n²)      |
| Space Complexity| O(1)       |

---

## 📎 Usage

```java
Solution sol = new Solution();
int[] nums = {-1, 2, 1, -4};
int target = 1;
System.out.println(sol.threeSumClosest(nums, target)); // Output: 2
```

---

## 📌 Notes

- This approach guarantees finding the closest sum due to full exploration using two pointers.
- Sorting the array is crucial for the pointer strategy to work correctly.

