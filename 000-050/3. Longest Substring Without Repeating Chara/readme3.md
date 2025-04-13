# Longest Substring Without Repeating Characters – Java Solution

This repository contains a Java implementation of the classic sliding window problem: **"Longest Substring Without Repeating Characters"** (LeetCode #3).

## 🧠 Problem Description

Given a string `s`, find the length of the longest substring without repeating characters.

**Example:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

## ✅ Approach: Sliding Window + HashMap

This solution uses the **sliding window** technique along with a **HashMap** to efficiently track characters and their most recent positions in the string.

### 🚀 Time and Space Complexity

- **Time Complexity:** O(n), where n is the length of the input string. Each character is visited at most twice.
- **Space Complexity:** O(min(n, m)), where m is the size of the character set (e.g., 26 for lowercase letters, 128 for ASCII).

---

## 📄 Code Explanation

```java
import java.util.HashMap;
import java.util.Map;
```

We import `HashMap` and `Map` to store characters and their last seen indices.

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
```

We define a class `Solution` and a method `lengthOfLongestSubstring` that takes a string `s` and returns the length of the longest substring without repeating characters.

```java
        Map<Character, Integer> charIndexMap = new HashMap<>();
```

This `HashMap` stores the **latest index** of each character encountered. This helps in quickly detecting duplicates within the current window.

```java
        int maxLength = 0;
        int start = 0;
```

- `maxLength` keeps track of the length of the longest valid substring found so far.
- `start` is the beginning index of the current window.

```java
        for (int end = 0; end < s.length(); end++) {
```

We iterate through the string using a pointer `end` that marks the end of the current window.

```java
            char currentChar = s.charAt(end);
```

We extract the current character.

```java
            if (charIndexMap.containsKey(currentChar)) {
                start = Math.max(start, charIndexMap.get(currentChar) + 1);
            }
```

If the character has already been seen, we **update the start** of the window. This ensures we skip over the repeated character and start fresh from the next position.

> ❗ Note: We use `Math.max` to avoid moving the `start` pointer backward.

```java
            charIndexMap.put(currentChar, end);
```

We update the map with the current character and its latest index.

```java
            maxLength = Math.max(maxLength, end - start + 1);
```

We update `maxLength` with the current window size if it is greater than any previously recorded value.

```java
        }

        return maxLength;
    }
}
```

We return the length of the longest substring without repeating characters.

---

## 🧪 Sample Test Cases

```java
Solution solution = new Solution();
System.out.println(solution.lengthOfLongestSubstring("abcabcbb")); // Output: 3
System.out.println(solution.lengthOfLongestSubstring("bbbbb"));    // Output: 1
System.out.println(solution.lengthOfLongestSubstring("pwwkew"));   // Output: 3
```

---

## 📌 Summary

- Uses a **sliding window** to maintain a valid substring without repeating characters.
- HashMap allows for **constant time** character position lookup.
- Efficient solution with optimal time complexity.

