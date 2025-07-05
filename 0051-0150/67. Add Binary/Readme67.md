
# ➕ LeetCode 67 – Add Binary

### 📄 Problem Statement:
Given two binary strings `a` and `b`, return their sum as a **binary string**.

---

### 🔍 Example

```

Input:  a = "11", b = "1"
Output: "100"

Input:  a = "1010", b = "1011"
Output: "10101"

````

---

### 💡 Approach: Two-Pointer Simulation with Carry

1. Start from the **end** of both strings (least significant bit).
2. Use two pointers `i` and `j` and a `carry` variable.
3. At each step:
   - Convert character digits `'0'` and `'1'` to integers.
   - Add corresponding bits and the carry.
   - Append `sum % 2` to the result.
   - Update `carry = sum / 2`.
4. After the loop, reverse the result.

---

### ✅ Java Code

```java
public class Solution {
    public String addBinary(String a, String b) {
        StringBuilder result = new StringBuilder();
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;

        // Traverse both strings from right to left
        while (i >= 0 || j >= 0 || carry > 0) {
            int sum = carry;
            if (i >= 0) sum += a.charAt(i--) - '0';
            if (j >= 0) sum += b.charAt(j--) - '0';
            result.append(sum % 2);
            carry = sum / 2;
        }

        return result.reverse().toString();
    }
}
````

---

### 🧠 Time & Space Complexity

| Operation        | Complexity                         |
| ---------------- | ---------------------------------- |
| Time Complexity  | O(max(n, m))                       |
| Space Complexity | O(max(n, m)) for the output string |

---

### 🚀 Why It Works

This solution mimics how binary addition is done by hand:

* Adds right to left
* Manages carry
* Appends digits in reverse

---

### 📌 Notes

* `StringBuilder` is efficient for appending.
* You don’t need to pad the strings — just process until both pointers and carry are done.

---

✅ This is a textbook example of **bitwise string simulation** and is great for interviews and practicing string manipulation.

```

---


