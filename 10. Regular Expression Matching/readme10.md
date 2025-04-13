# Regular Expression Matching – Java Solution

This repository contains a dynamic programming solution in Java for solving **LeetCode #10 – Regular Expression Matching**.

---

## 🧠 Problem Statement

Implement regular expression matching with support for `'.'` and `'*'`:

- `'.'` Matches any single character.
- `'*'` Matches zero or more of the preceding element.

The matching should cover **the entire input string** (not partial).

---

## 🔍 Example Inputs & Outputs

```text
Input: s = "aa", p = "a"
Output: false

Input: s = "aa", p = "a*"
Output: true

Input: s = "mississippi", p = "mis*is*p*."
Output: false
```

---

## ✅ Approach: Dynamic Programming (Bottom-Up)

### 🧮 Idea

We use a **2D DP table** where:

- `dp[i][j]` is `true` if `s[0..i-1]` matches `p[0..j-1]`
- Initialize `dp[0][0] = true` (empty string matches empty pattern)
- Handle `*` by checking:
  - **Zero occurrences** of the preceding char: `dp[i][j] = dp[i][j - 2]`
  - **One or more occurrences**: check if current `s[i-1]` matches the character before `*`, then:  
    `dp[i][j] |= dp[i - 1][j]`
- If current pattern char is `.` or matches the character from string, do:  
  `dp[i][j] = dp[i - 1][j - 1]`

---

## 📄 Code Explanation

```java
public boolean isMatch(String s, String p) {
    int m = s.length(), n = p.length();
    boolean[][] dp = new boolean[m + 1][n + 1];
    dp[0][0] = true;
```

Initialize the DP table. `dp[0][0] = true` means empty string matches empty pattern.

```java
    for (int j = 1; j <= n; j++) {
        if (p.charAt(j - 1) == '*' && j >= 2) {
            dp[0][j] = dp[0][j - 2];
        }
    }
```

Pre-fill the first row where the string is empty, but the pattern can still match if it includes `*` for zero occurrences.

```java
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
```

Traverse the DP table, character by character.

```java
            if (p.charAt(j - 1) == '*') {
                if (j >= 2) {
                    dp[i][j] = dp[i][j - 2];
                    if (p.charAt(j - 2) == '.' || p.charAt(j - 2) == s.charAt(i - 1)) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                }
            }
```

When `*` is found:
- Try **zero** occurrence by checking `dp[i][j - 2]`
- Try **one or more** if preceding character matches current character in `s` or is a dot (`.`)

```java
            else {
                if (p.charAt(j - 1) == '.' || p.charAt(j - 1) == s.charAt(i - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
            }
```

When current character is `.` or matches the character in `s`, move diagonally in the table.

```java
        }
    }
    return dp[m][n];
}
```

Return the result in `dp[m][n]`, meaning: does entire string match entire pattern?

---

## 💡 Visual Example

Let’s say:

```text
s = "aab", p = "c*a*b"
```

Pattern breakdown:

- `'c*'` matches zero `'c'`
- `'a*'` matches `'aa'`
- `'b'` matches `'b'`

✅ Match successful.

---

## 🧪 Sample Test Cases

```java
Solution s = new Solution();
System.out.println(s.isMatch("aa", "a"));         // false
System.out.println(s.isMatch("aa", "a*"));        // true
System.out.println(s.isMatch("ab", ".*"));        // true
System.out.println(s.isMatch("mississippi", "mis*is*p*.")); // false
System.out.println(s.isMatch("aab", "c*a*b"));    // true
```

---

## 📌 Summary

- Uses **2D dynamic programming**.
- Handles `*` carefully to account for both **zero** and **multiple** matches.
- Time Complexity: **O(m × n)**
- Space Complexity: **O(m × n)**



