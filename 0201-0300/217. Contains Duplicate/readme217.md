# LeetCode 217 - Contains Duplicate

This repository contains Java solutions and explanations for **LeetCode Problem 217: Contains Duplicate**.

## 📝 Problem Statement

> Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

### 🔍 Example

```java
Input: nums = [1,2,3,1]
Output: true

Input: nums = [1,2,3,4]
Output: false
````

---

## 💡 Approach 1: Brute Force (Nested Loops)

### 👨‍💻 Idea

* Loop through each element in the array.
* For every current element, run a second loop starting from the next index.
* If any duplicate is found, return `true`.

### 🧠 Time & Space Complexity

| Metric | Value |
| ------ | ----- |
| Time   | O(n²) |
| Space  | O(1)  |

### ✅ Java Code

```java
public boolean containsDuplicateBruteForce(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] == nums[j]) {
                return true;
            }
        }
    }
    return false;
}
```

### 📌 Notes

* This solution is simple to implement.
* Not suitable for large arrays due to its quadratic time complexity.

---

## 💡 Approach 2: Hash Table (HashMap or HashSet)

### 👨‍💻 Idea

* Traverse the array and store each element in a hash-based structure.
* Check if the element already exists during insertion.
* If it does, return `true`; otherwise, keep adding.

> ❗ You **do not** need to count frequencies — just tracking existence is enough.

### 🧠 Time & Space Complexity

| Metric | Value |
| ------ | ----- |
| Time   | O(n)  |
| Space  | O(n)  |

### ✅ Java Code (Using HashSet)

```java
import java.util.HashSet;
import java.util.Set;

public boolean containsDuplicateHashSet(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (seen.contains(num)) return true;
        seen.add(num);
    }
    return false;
}
```

### 📌 Notes

* This is the **optimal and most recommended** solution.
* Provides a great balance of performance and readability.

---

## 📊 Comparison Table

| Approach             | Time Complexity | Space Complexity | Suitable For           | Performance             |
| -------------------- | --------------- | ---------------- | ---------------------- | ----------------------- |
| Brute Force          | O(n²)           | O(1)             | Small input sizes      | ❌ Slow                  |
| HashSet              | O(n)            | O(n)             | All input sizes        | ✅ Best                  |
| HashMap (with count) | O(n)            | O(n)             | Slightly overkill here | ✅ Works but unnecessary |

---

## ✅ Recommendation

Use the **HashSet approach** for best performance and simplicity. Only use the brute-force method for understanding or when input size is guaranteed to be small.

---
