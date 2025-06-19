
## 📝 **Problem Statement – LeetCode 229**

> Given an integer array of size `n`, find **all** elements that appear **more than ⌊n / 3⌋ times**.

### 🧠 Key Insight:

There can be **at most 2 elements** that appear more than ⌊n / 3⌋ times.

---

## ✅ **Approach: Extended Boyer–Moore Voting Algorithm**

We modify the classic Boyer–Moore algorithm to handle two potential candidates instead of one.

---

### 🔄 **Steps:**

1. **First pass:** Find up to 2 candidates using a similar vote system.
2. **Second pass:** Count actual occurrences of the candidates.
3. Return the ones that occur more than `n / 3` times.

---

### 📦 Java Code:

```java
public List<Integer> majorityElement(int[] nums) {
    int candidate1 = 0, candidate2 = 0;
    int count1 = 0, count2 = 0;

    // Step 1: Find possible candidates
    for (int num : nums) {
        if (num == candidate1) {
            count1++;
        } else if (num == candidate2) {
            count2++;
        } else if (count1 == 0) {
            candidate1 = num;
            count1 = 1;
        } else if (count2 == 0) {
            candidate2 = num;
            count2 = 1;
        } else {
            count1--;
            count2--;
        }
    }

    // Step 2: Verify counts
    count1 = 0;
    count2 = 0;
    for (int num : nums) {
        if (num == candidate1) count1++;
        else if (num == candidate2) count2++;
    }

    // Step 3: Collect valid candidates
    List<Integer> result = new ArrayList<>();
    int n = nums.length;
    if (count1 > n / 3) result.add(candidate1);
    if (count2 > n / 3) result.add(candidate2);

    return result;
}
```

---

### ⏱️ Time and Space Complexity:

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |

---

### 📌 Example:

**Input:**
`nums = [3, 2, 3]`
**Output:**
`[3]`

**Input:**
`nums = [1, 1, 1, 3, 3, 2, 2, 2]`
**Output:**
`[1, 2]`

---


