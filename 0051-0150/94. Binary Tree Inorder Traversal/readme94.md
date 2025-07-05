
# 🌳 LeetCode 94 – Binary Tree Inorder Traversal

### 🧩 Problem Statement:
Given the `root` of a binary tree, return the **inorder traversal** of its nodes' values.

> Inorder traversal visits nodes in the order:  
> **Left ➝ Root ➝ Right**

---

### 📥 Example:

**Input:**
```text
    1
     \
      2
     /
    3
````

**Output:**

```
[1, 3, 2]
```

---

## 💡 Approach 1: Recursive DFS (Depth-First Search)

* Traverse left subtree recursively
* Visit the current node
* Traverse right subtree recursively

---

### ✅ Java Code (Recursive)

```java
import java.util.*;

class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }

    private void inorder(TreeNode node, List<Integer> result) {
        if (node == null) return;
        inorder(node.left, result);   // Visit left
        result.add(node.val);         // Visit node
        inorder(node.right, result);  // Visit right
    }
}
```

---

### 🧠 Time & Space Complexity:

| Metric           | Value                                                |
| ---------------- | ---------------------------------------------------- |
| Time Complexity  | O(n) (every node is visited once)                    |
| Space Complexity | O(n) worst-case due to recursion stack + output list |

---

## 🚀 Alternative Approaches

### 🔁 Approach 2: Iterative Inorder (using Stack)

If recursion is not allowed or stack overflow is a concern (deep trees), use an explicit stack.

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode current = root;

    while (current != null || !stack.isEmpty()) {
        while (current != null) {
            stack.push(current);
            current = current.left;
        }
        current = stack.pop();
        result.add(current.val);
        current = current.right;
    }

    return result;
}
```

### 🔥 Advanced: Morris Traversal (O(1) Space)

* Temporarily modifies the tree.
* No recursion or stack.
* O(n) time, **O(1) space**.
* Useful in memory-constrained environments.

Let me know if you’d like this version too!

---

## 🧪 Test Cases

```text
Input: root = [1,null,2,3]
Output: [1,3,2]

Input: root = []
Output: []

Input: root = [1]
Output: [1]
```

---

## 📌 Summary

| Approach      | Time | Space | Notes                     |
| ------------- | ---- | ----- | ------------------------- |
| Recursive DFS | O(n) | O(n)  | Simple, readable          |
| Iterative     | O(n) | O(n)  | Safer for deep trees      |
| Morris        | O(n) | O(1)  | No stack, but alters tree |

---


