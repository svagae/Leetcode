## 🧮 LeetCode 169 - Majority Element

This repository contains a Java solution for **LeetCode Problem 169: Majority Element** using a Hash Map to count frequencies.

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
`nums = [3, 2, 3]`
**Output:**
`3`

---

### 💡 Approach: Using Hash Map 

1. Create a hash map to store each number and how many times it appears.
2. Traverse the array once to fill the map.
3. Traverse the map to find the number that appears more than `n / 2` times.

---

### 🧠 Why It Works

* The problem guarantees that a majority element exists.
* By counting occurrences and checking if any value exceeds `n / 2`, we can directly find the correct element.

---

### 📦 Java Code

```java
public int majorityElement(int[] nums) {
    Map<Integer, Integer> countMap = new HashMap<>();
    int n = nums.length;

    for (int num : nums) {
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
    }

    for (Map.Entry<Integer, Integer> entry : countMap.entrySet()) {
        if (entry.getValue() > n / 2) {
            return entry.getKey();
        }
    }

    return -1; 
}
```

---

### ⏱️ Time & Space Complexity

* **Time Complexity:** O(n)
* **Space Complexity:** O(n)

---

### ✅ Strengths

* Simple and easy to understand.
* Straightforward to implement using Java's `HashMap`.

---

### 🔄 Alternative (More Efficient) Method

For optimal space usage, check out the **Boyer–Moore Voting Algorithm**, which solves the same problem in `O(1)` space.

---


