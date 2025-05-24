# Letter Combinations of a Phone Number

## Problem Description

Given a string containing digits from `2` to `9`, return all possible letter combinations that the number could represent. Return the answer in **any order**.

Each digit maps to a set of letters just like on the telephone buttons.

> This is LeetCode Problem [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

---

## Example

### Input:

```text
digits = "23"
```

### Output:

```text
["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

---

## Approach

We solve the problem using a **backtracking** algorithm, which is ideal for exploring all possible combinations by building a solution incrementally and abandoning it ("backtracking") as soon as it becomes invalid or complete.

---

## Strategy: Backtracking with a Phone Map

1. **Map digits to letters** using a `HashMap` according to a standard phone keypad.
2. If the input `digits` is empty, return an empty list.
3. Use a **recursive backtracking function** to:

   * Build up a current combination of letters.
   * Once the current combination reaches the length of the input digits, it's considered complete and added to the result list.
   * Otherwise, continue appending each possible letter for the current digit and recurse to the next index.
4. Use `StringBuilder` to efficiently build and backtrack combinations.

---

## Code

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) {
            return new ArrayList<>();
        }

        Map<Character, String> phoneMap = new HashMap<>();
        phoneMap.put('2', "abc");
        phoneMap.put('3', "def");
        phoneMap.put('4', "ghi");
        phoneMap.put('5', "jkl");
        phoneMap.put('6', "mno");
        phoneMap.put('7', "pqrs");
        phoneMap.put('8', "tuv");
        phoneMap.put('9', "wxyz");

        List<String> result = new ArrayList<>();
        backtrack(digits, 0, new StringBuilder(), result, phoneMap);
        return result;
    }

    private void backtrack(String digits, int index, StringBuilder currentCombination,
                           List<String> result, Map<Character, String> phoneMap) {
        if (index == digits.length()) {
            result.add(currentCombination.toString());
            return;
        }

        char digit = digits.charAt(index);
        String letters = phoneMap.get(digit);

        for (int i = 0; i < letters.length(); i++) {
            currentCombination.append(letters.charAt(i));
            backtrack(digits, index + 1, currentCombination, result, phoneMap);
            currentCombination.deleteCharAt(currentCombination.length() - 1); // backtrack
        }
    }
}
```

---

## Time & Space Complexity

* **Time Complexity:** O(3ⁿ × 4ᵐ), where `n` is the number of digits that map to 3 letters (`2`, `3`, `4`, `5`, `6`, `8`) and `m` is the number of digits that map to 4 letters (`7`, `9`).
* **Space Complexity:** O(n), where `n` is the maximum depth of the recursion stack (equal to the number of digits).

---

## Summary

This backtracking solution efficiently explores all valid letter combinations by:

* Mapping digits to characters,
* Recursively building combinations,
* And reverting changes after exploring each branch.

It is clean, scalable, and widely used for problems involving permutations and combinations.

