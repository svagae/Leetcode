# 🧠 LeetCode 136: Single Number 

## 📝 Problem Statement

> Given a **non-empty array** of integers, every element appears **twice** except for one.
> Find that single one.
>
> ✅ You must implement a solution with **linear runtime complexity (O(n))**.
> ✅ Try to use **only constant extra space**.

---

## 🔢 Example

```text
Input:  [4, 1, 2, 1, 2]
Output: 4
Explanation: 4 appears once, all others appear twice.
```

---

## ✅ Approach 1: HashMap to Count Frequencies

### 🔍 Idea

We scan the array and count how many times each number appears using a `HashMap<Integer, Integer>`. Then, we scan the map to find the key with value = 1.

### 💻 Java Code

```java
public int singleNumber(int[] nums) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }

    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        if (entry.getValue() == 1) {
            return entry.getKey();
        }
    }

    return -1; // fallback
}
```

### 📊 Time & Space Complexity

* **Time:** O(n) — one loop for array, one loop for map
* **Space:** O(n) — stores all elements and their counts

### 📌 Example Trace

For input: `[2, 3, 2, 4, 4]`

1. **After filling map:**

   ```
   {
       2: 2,
       3: 1,
       4: 2
   }
   ```

2. **Scanning map:**

   * 2 → not valid
   * 3 → ✅ appears once → **return 3**
   * Done

### ✅ Pros

* Easy to understand
* Works for any "k times except one" type of problem

### ❌ Cons

* **Violates space constraint** (problem wants constant space)
* Uses extra memory to track counts

---

## ✅ Approach 2: XOR (Bit Manipulation) — Optimal

### 🧠 Idea

Using XOR’s properties:

* `a ^ a = 0`
* `a ^ 0 = a`
* XOR is **commutative** and **associative**

So when you XOR all numbers together, pairs cancel out and the result is the **unique number**.

### 💻 Java Code

```java
public int singleNumber(int[] nums) {
    int result = 0;

    for (int num : nums) {
        result ^= num;
    }

    return result;
}
```

### 📊 Time & Space Complexity

* **Time:** O(n) — one pass
* **Space:** O(1) — no extra memory used

### 🔁 Example Trace

Input: `[4, 1, 2, 1, 2]`

```text
Step-by-step XOR:

result = 0
result = 0 ^ 4 = 4
result = 4 ^ 1 = 5
result = 5 ^ 2 = 7
result = 7 ^ 1 = 6
result = 6 ^ 2 = 4

Final result = 4
```

### ✅ Pros

* Constant space
* Very fast
* Satisfies problem constraints

### ❌ Cons

* Might seem non-intuitive at first
* Doesn’t work if numbers appear more than twice (unless XOR generalization is used)

---

## ⚖️ Comparison Table

| Feature           | HashMap Approach     | XOR Approach (Optimal)  |
| ----------------- | -------------------- | ----------------------- |
| Time Complexity   | O(n)                 | O(n)                    |
| Space Complexity  | O(n)                 | O(1)                    |
| Meets Constraints | ❌ (extra space used) | ✅ (constant space)      |
| Code Simplicity   | Easy                 | Medium (needs XOR idea) |
| General Use Case  | Works for "k times"  | Only works with 2 times |

---

## ✅ Conclusion

* If you're solving **coding interview problems**, the **XOR solution is preferred** due to its speed and space efficiency.
* If you're just practicing or allowed extra space, the **HashMap solution is simpler and more general**.

