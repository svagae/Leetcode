

## 🧮 LeetCode 169 - Majority Element

This repository contains a Java solution for **LeetCode Problem 169: Majority Element** using the **Boyer–Moore Majority Vote Algorithm** for optimal performance.

---

### 📝 Problem Statement

> Given an array `nums` of size `n`, return the **majority element**.
>
> The majority element is the element that appears **more than ⌊n / 2⌋ times**.
>
> You may assume that the majority element always exists in the array.

---

### 📥 Example

**Input:**
`nums = [2, 2, 1, 1, 1, 2, 2]`
**Output:**
`2`

---

### 🚀 Approach: Boyer–Moore Voting Algorithm

This algorithm works by:

1. Maintaining a `candidate` and a `count`.
2. Resetting the candidate when the count reaches 0.
3. Increasing count if the current number equals the candidate; otherwise, decreasing it.
4. Because the majority element appears more than `n/2` times, it always survives at the end.

---

### 📦 Java Code

```java
public int majorityElement(int[] nums) {
    int count = 0;
    int candidate = 0;

    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }

        if (num == candidate) {
            count++;
        } else {
            count--;
        }
    }

    return candidate;
}
```

---

### ⏱️ Time & Space Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)

---

### ✅ Why Boyer–Moore?

* **No extra memory** is needed — only two variables.
* Efficient even for large arrays.
* Guaranteed to work since a majority element is always present.

---

### 📄 License

This project is licensed under the MIT License.

---
