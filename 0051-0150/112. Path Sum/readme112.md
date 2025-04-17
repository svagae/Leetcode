# 🌳 LeetCode 112: Path Sum – Java Solution

## 📘 Problem Description

Given the root of a binary tree and an integer `targetSum`, determine if the tree has a **root-to-leaf** path such that the sum of the values along the path equals `targetSum`.

A **leaf** is a node with no children.

---

## 🧪 Example

**Input:**

```text
root = [5,4,8,11,null,13,4,7,2,null,null,null,1]
targetSum = 22
```

**Output:**

```text
true
```

**Explanation:**  
The path `5 → 4 → 11 → 2` sums to `22`.

---

## ✅ Solution Overview

We use a **recursive Depth-First Search (DFS)** to explore all paths from the root to the leaves.  
At each step, we subtract the current node’s value from the target sum.  
If we reach a leaf and the remaining sum equals the node's value, we found a valid path.

---

## 🧠 Algorithm Steps

1. **Base Case – Null Node:**  
   Return `false` if the node is `null`.

2. **Base Case – Leaf Node:**  
   Return `true` if it’s a leaf and `targetSum == node.val`.

3. **Recursive Case – Internal Node:**  
   - Subtract `node.val` from `targetSum`
   - Recursively check left and right subtrees with the updated `targetSum`

---

## ⏱️ Time & Space Complexity

- **Time Complexity:** `O(n)` – Each node is visited once
- **Space Complexity:** `O(h)` – `h` is the tree height  
  - `O(log n)` for balanced trees  
  - `O(n)` for skewed trees (linked-list shaped)

---

## 📄 Java Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        
        if (root.left == null && root.right == null)
            return targetSum == root.val;
        
        int remainingSum = targetSum - root.val;
        return hasPathSum(root.left, remainingSum) || hasPathSum(root.right, remainingSum);
    }
}
