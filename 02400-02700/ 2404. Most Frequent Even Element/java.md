

## 🔢 LeetCode 2404 - Most Frequent Even Element

This repository contains a Java solution for **LeetCode Problem 2404**: *Most Frequent Even Element* using a **HashMap-based approach** to count frequencies.

---

### 📝 Problem Statement

> Given an integer array `nums`, return **the most frequent even element**.
> If there is a tie in frequency, return the **smallest even element**.
> If no even elements exist, return **-1**.

---

### 🧪 Example 1

```txt
Input:  nums = [0,1,2,2,4,4,1]
Output: 2

Explanation:
Even elements = [0, 2, 2, 4, 4]
Frequencies: 0 → 1, 2 → 2, 4 → 2
2 and 4 tie, but 2 is smaller → return 2
```

---

### 💡 Approach 1: HashMap + Frequency Count

This approach uses a `HashMap` to:

* Count how often each even number appears.
* Then find the most frequent one.
* In case of a tie, return the smallest one.

---

### 📦 Java Code

```java
public int mostFrequentEven(int[] nums) {
    Map<Integer, Integer> freq = new HashMap<>();

    for (int num : nums) {
        if (num % 2 == 0) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }
    }

    int maxFreq = 0;
    int result = -1;

    for (Map.Entry<Integer, Integer> entry : freq.entrySet()) {
        int key = entry.getKey();
        int value = entry.getValue();

        if (value > maxFreq || (value == maxFreq && key < result)) {
            maxFreq = value;
            result = key;
        }
    }

    return result;
}
```

---

### 🧠 Explanation

1. Loop through each number:

   * Only count it if it's even.
   * Use `getOrDefault(num, 0) + 1` to update frequency.
2. Track:

   * `maxFreq`: the highest frequency found.
   * `result`: the smallest even number with the highest frequency.
3. Return `result`, or `-1` if no even numbers were found.

---

### ⏱️ Complexity Analysis

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(n)  |

* `n` is the number of elements in the input array.
* Worst-case space is O(n) if all elements are unique and even.

---

### ✅ Strengths of This Approach

* Simple and intuitive.
* Easy to implement and understand.
* Uses standard `HashMap` features (`getOrDefault`) to keep code concise.

---

### 📚 Java Feature Used: `getOrDefault()`

```java
freq.getOrDefault(num, 0) + 1
```

Returns the value mapped to `num`, or `0` if `num` is not in the map — avoids null checks and keeps code cleaner.

---

### 🔄 Alternative Approaches

* **Array-based Counting**: More space-efficient if the input range is known (e.g., 0 to 100000).
* **Boyer–Moore?** Not suitable — it’s only for elements appearing more than n/2 times, not general frequency counting.

---

