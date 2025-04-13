# Longest Palindromic Substring – Java Solution

This repository contains a Java solution for finding the **longest palindromic substring** in a given string `s`.

---

## 🧠 Problem Description

Given a string `s`, return the **longest palindromic substring** in `s`.

### 🔍 Examples

```text
Input: "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

Input: "cbbd"
Output: "bb"
```

---

## ✅ Approach: Expand Around Center

### 🔧 Core Idea

A palindrome mirrors around its center. The center can either be:

- A single character (odd length palindrome).
- A gap between two characters (even length palindrome).

We expand around each possible center and track the longest valid palindrome.

---

## 📈 Time and Space Complexity

- **Time Complexity:** O(n²), where n is the length of the string. Each expansion takes O(n) in the worst case.
- **Space Complexity:** O(1), since we use only constant extra space.

---

## 📄 Code Explanation

```java
public class Solution {
    public String longestPalindrome(String s) {
```

Defines the class `Solution` and the main method `longestPalindrome`.

```java
        if (s == null || s.length() < 1) 
            return "";
```

Handles edge cases for empty or null strings.

```java
        int start = 0, end = 0;
```

`start` and `end` will hold the indices of the longest palindrome found.

```java
        for (int i = 0; i < s.length(); i++) {
```

Loop over each character in the string, treating it as a potential **center**.

```java
            int len1 = expandAroundCenter(s, i, i); 
            int len2 = expandAroundCenter(s, i, i + 1);  
```

- `len1` checks for an **odd-length** palindrome (center is `i`).
- `len2` checks for an **even-length** palindrome (center is between `i` and `i + 1`).

```java
            int len = Math.max(len1, len2);
```

Pick the longer of the two.

```java
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
```

Update the `start` and `end` indices if a longer palindrome is found.

```java
        return s.substring(start, end + 1);
```

Return the substring representing the longest palindrome.

---

## 🔄 Helper Method: `expandAroundCenter`

```java
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
```

- Expands outward as long as characters at `left` and `right` match.
- When the loop ends, the length of the palindrome is `right - left - 1`.

---

## 🧪 Sample Test Cases

```java
Solution s = new Solution();
System.out.println(s.longestPalindrome("babad"));  // Output: "bab" or "aba"
System.out.println(s.longestPalindrome("cbbd"));   // Output: "bb"
System.out.println(s.longestPalindrome("a"));      // Output: "a"
System.out.println(s.longestPalindrome("ac"));     // Output: "a" or "c"
```

---

## 📌 Summary

- The solution checks all potential palindrome centers.
- Efficient and elegant for short to medium strings.
- Doesn't require extra space like dynamic programming.
