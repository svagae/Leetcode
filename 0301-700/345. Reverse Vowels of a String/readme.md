

# 🔄 LeetCode 345 – Reverse Vowels of a String

## ✅ Problem Statement

Given a string `s`, **reverse only the vowels** of the string and return the result.

---

## 🧪 Examples

### Example 1

```txt
Input:  "hello"
Output: "holle"
```

### Example 2

```txt
Input:  "leetcode"
Output: "leotcede"
```

---

## 🎯 Goal

Reverse only the **vowel characters** in the string. Keep all other characters in their original positions.

**Vowels (case-insensitive):**
`a, e, i, o, u` and `A, E, I, O, U`

---

## 🧠 Approach 1: Two-Pass Strategy (Your Idea)

### Step-by-step Plan:

1. **Extract all vowels** from the string into a temporary string (or list).
2. **Reverse** that string of vowels.
3. Loop again through the original string and whenever a vowel is found, **replace it with the next character** from the reversed string.

---

## 🔧 Java Code

```java
class Solution {
    public String reverseVowels(String s) {
        String vowels = "aeiouAEIOU";  // All possible vowels
        StringBuilder vowelOnly = new StringBuilder();

        // Step 1: Collect all vowels
        for (char c : s.toCharArray()) {
            if (vowels.indexOf(c) != -1) { // Check if c is a vowel
                vowelOnly.append(c);       // Save it
            }
        }

        // Step 2: Reverse the vowels
        vowelOnly.reverse();

        // Step 3: Rebuild the string
        StringBuilder result = new StringBuilder();
        int vowelIndex = 0;

        for (char c : s.toCharArray()) {
            if (vowels.indexOf(c) != -1) {
                result.append(vowelOnly.charAt(vowelIndex++)); // Replace with reversed vowel
            } else {
                result.append(c); // Keep the original non-vowel
            }
        }

        return result.toString();
    }
}
```

---

## 🔍 Explanation of Key Functions

### 🧩 `toCharArray()`

* Converts a string like `"hello"` into a character array: `['h', 'e', 'l', 'l', 'o']`
* Useful for iterating character-by-character

### 🧩 `indexOf(c)`

* Searches for character `c` in the string
* If `c` is found, it returns the **index**
* If not found, it returns `-1`

```java
vowels.indexOf('e') → 1   ✅ vowel
vowels.indexOf('x') → -1  ❌ not a vowel
```

### 🧩 `append()`

* Used with `StringBuilder` to add characters or strings

```java
StringBuilder sb = new StringBuilder();
sb.append('a'); // sb = "a"
sb.append('b'); // sb = "ab"
```

---

## 📊 Time and Space Complexity

| Metric | Value                           |
| ------ | ------------------------------- |
| Time   | O(n)                            |
| Space  | O(n) (extra storage for vowels) |

---

## 🚀 Bonus: In-Place Two-Pointer Solution (More Efficient)

```java
class Solution {
    public String reverseVowels(String s) {
        char[] arr = s.toCharArray();
        String vowels = "aeiouAEIOU";
        int left = 0, right = arr.length - 1;

        while (left < right) {
            while (left < right && vowels.indexOf(arr[left]) == -1) left++;
            while (left < right && vowels.indexOf(arr[right]) == -1) right--;

            // Swap vowels
            char temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;

            left++;
            right--;
        }

        return new String(arr);
    }
}
```

### ✅ Benefits:

* Does **everything in one pass**
* Uses **O(1) space** since no vowel string is needed
* Faster in practice

---

## ✏️ Example Walkthrough

Input: `"hello"`

Step-by-step:

* Vowels = `"eo"` → reversed = `"oe"`
* Rebuild:

  * `'h'` → `'h'`
  * `'e'` → `'o'`
  * `'l'` → `'l'`
  * `'l'` → `'l'`
  * `'o'` → `'e'`

Result = `"holle"`

---

## 🧠 Summary

| Feature               | Two-Pass | Two-Pointer       |
| --------------------- | -------- | ----------------- |
| Time Complexity       | O(n)     | O(n)              |
| Space Complexity      | O(n)     | O(1) ✅            |
| Code Simplicity       | Easier   | Slightly trickier |
| In-place modification | ❌        | ✅                 |

---

## 📘 Related Problems

* [LeetCode 344 – Reverse String](https://leetcode.com/problems/reverse-string/)
* [LeetCode 151 – Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
* [LeetCode 557 – Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii/)

---


