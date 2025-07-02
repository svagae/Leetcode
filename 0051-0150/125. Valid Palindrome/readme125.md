## 🧪 Valid Palindrome Problem – Java Solution

### 🔍 Problem Statement

Determine whether a given string is a **valid palindrome**, ignoring:

* Whitespace characters
* Punctuation/symbols (e.g., `:,!@`)
* Case differences (`'A' == 'a'`)

---

## ✅ Solution Approaches

### 🟩 1. Preprocessing + Two-Pointer Approach

This method **cleans the string first**, removing:

* All whitespace characters (`\\s+`)
* All non-alphanumeric symbols (`[^a-zA-Z0-9\\s]`)
* Converts to lowercase

Then, it applies the **two-pointer technique** to check if the cleaned string is a palindrome.

#### 🔧 Java Code:

```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null) return true;

        // Clean the string: remove spaces, symbols, and lowercase it
        String m = s.replaceAll("\\s+", "")
                    .toLowerCase()
                    .replaceAll("[^a-zA-Z0-9\\s]", "");

        int l = 0;
        int r = m.length() - 1;

        while (l <= r) {
            if (m.charAt(l) != m.charAt(r)) return false;
            l++;
            r--;
        }
        return true;
    }
}
```

#### ✅ Pros:

* Easy to read and understand
* Good for short strings or when performance isn't critical

#### ⚠️ Cons:

* **Extra memory** used for cleaned string
* Slightly slower due to regex operations

---

### 🟩 2. In-Place Two-Pointer Approach (Most Optimal)

This approach avoids extra memory by **scanning the original string** with two pointers and skipping non-alphanumeric characters on the fly.

#### 🔧 Java Code:

```java
class Solution {
    public boolean isPalindrome(String s) {
        if (s == null) return true;

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;

            if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

#### ✅ Pros:

* **Time Complexity:** O(n)
* **Space Complexity:** O(1)
* Skips all unnecessary characters in-place
* Optimal for performance and large strings

#### ⚠️ Cons:

* Slightly more complex logic due to nested pointer checks

---

## 🚀 Example Inputs & Outputs

| Input                              | Output  |
| ---------------------------------- | ------- |
| `"A man, a plan, a canal: Panama"` | `true`  |
| `"race a car"`                     | `false` |
| `" "`                              | `true`  |
| `"0P"`                             | `false` |

---

## 🧠 Summary

| Approach                  | Time | Space | Suitable For                    |
| ------------------------- | ---- | ----- | ------------------------------- |
| Preprocess + Two Pointers | O(n) | O(n)  | Simple, readable, small inputs  |
| In-Place Two Pointers     | O(n) | O(1)  | Optimal, memory-sensitive cases |

Use the second approach when efficiency matters (interviews or production), and the first for clarity in learning or small-scale problems.

---

