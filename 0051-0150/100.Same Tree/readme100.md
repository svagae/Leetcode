# 🌳 LeetCode 100: Same Tree – Java Solution

## 📘 Problem Description

Given the roots of two binary trees `p` and `q`, write a function to check if they are the **same tree**.

Two binary trees are considered the same if:
- They are **structurally identical**, and
- The **nodes have the same values**.

---

## 🧪 Example

**Input:**
```text
p = [1,2,3], q = [1,2,3]
```

**Output:**
```text
true
```

**Input:**
```text
p = [1,2], q = [1,null,2]
```

**Output:**
```text
false
```

---

## ✅ Solution Overview

We use a **recursive approach** to simultaneously traverse both trees and compare:
- If both nodes are `null`, we return `true`.
- If only one of them is `null`, or if the values differ, we return `false`.
- Otherwise, we recursively check both left and right subtrees.

---

## 🧠 Algorithm Steps

1. **Base Case:**
   - If both nodes `p` and `q` are `null`, return `true`.
   - If one is `null` and the other is not, return `false`.
   - If `p.val != q.val`, return `false`.

2. **Recursive Case:**
   - Recursively check:
     - `isSameTree(p.left, q.left)`
     - `isSameTree(p.right, q.right)`
   - Return `true` only if both recursive calls return `true`.

---

## ⏱️ Time & Space Complexity

- **Time Complexity:** `O(n)`, where `n` is the number of nodes in the smaller tree.
- **Space Complexity:** `O(h)`, where `h` is the height of the tree due to recursive call stack.  
  - Balanced tree: `O(log n)`  
  - Skewed tree: `O(n)`

---

## 📄 Java Code

```java
// Definition for a binary tree node.
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) return true;
        if (p == null || q == null) return false;
        if (p.val != q.val) return false;
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```

---

## 🧪 Test Cases

| p                         | q                         | Output |
|--------------------------|---------------------------|--------|
| `[1,2,3]`                | `[1,2,3]`                 | `true` |
| `[1,2]`                  | `[1,null,2]`              | `false`|
| `[]`                     | `[]`                      | `true` |
| `[1,2,1]`                | `[1,1,2]`                 | `false`|

