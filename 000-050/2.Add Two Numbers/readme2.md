# ➕ Add Two Numbers (Linked List)

## 📝 Overview

This repository contains a Java solution to the popular **Add Two Numbers** problem from LeetCode.

The problem involves adding two numbers represented by **linked lists**, where each node contains a single digit, and the digits are stored in **reverse order**. The solution returns the sum as a new linked list in the same reversed format.

---

## 📌 Problem Statement

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. **Add the two numbers** and return the sum as a linked list.

**You may assume the two numbers do not contain any leading zero, except the number 0 itself.**

### Example

**Input:**
```
l1 = [2,4,3]
l2 = [5,6,4]
```

**Output:**
```
[7,0,8]
```

**Explanation:**  
342 + 465 = 807, and the result is represented as 7 → 0 → 8.

---

## 📄 Code

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0); 
        ListNode current = dummy; 
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry; 

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next; 
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next; 
            }

            carry = sum / 10; 
            current.next = new ListNode(sum % 10);
            current = current.next; 
        }

        return dummy.next; 
    }
}
```

---

## 🧠 Explanation

### 👣 Step-by-step Breakdown

1. **Create Dummy Node**:
   ```java
   ListNode dummy = new ListNode(0);
   ListNode current = dummy;
   ```
   - `dummy` is a placeholder to simplify list operations.
   - `current` will traverse and build the result list.

2. **Initialize Carry**:
   ```java
   int carry = 0;
   ```
   - Used to store carry-over during addition (e.g., 7 + 5 = 12, carry = 1).

3. **Loop While Nodes Exist or Carry Remains**:
   ```java
   while (l1 != null || l2 != null || carry != 0) {
   ```
   - Continue while any list has remaining nodes or carry is non-zero.

4. **Start with the Carry**:
   ```java
   int sum = carry;
   ```

5. **Add Value from l1 If Not Null**:
   ```java
   if (l1 != null) {
       sum += l1.val;
       l1 = l1.next;
   }
   ```

6. **Add Value from l2 If Not Null**:
   ```java
   if (l2 != null) {
       sum += l2.val;
       l2 = l2.next;
   }
   ```

7. **Update Carry and Add Node**:
   ```java
   carry = sum / 10;
   current.next = new ListNode(sum % 10);
   current = current.next;
   ```
   - Extract the digit using `sum % 10`.
   - Carry will be `sum / 10` for next iteration.

8. **Return the Result**:
   ```java
   return dummy.next;
   ```
   - Skips the dummy head to return the actual result.

---

## 📊 Time and Space Complexity

| Metric           | Complexity  |
|------------------|-------------|
| **Time**         | O(max(n, m)) where `n` and `m` are the lengths of `l1` and `l2` |
| **Space**        | O(max(n, m)) for the output list |

---

## 🚀 Example Walkthrough

**Input**:  
`l1 = [9, 9]` → represents 99  
`l2 = [1]` → represents 1

**Process**:  
- 9 + 1 = 10 → add `0`, carry `1`  
- 9 + 0 + 1 = 10 → add `0`, carry `1`  
- No nodes left, but carry = 1 → add `1`

**Output**: `[0, 0, 1]` → represents 100

---

## 🧪 Usage

```java
// Assuming you already have l1 and l2 as ListNode instances
Solution sol = new Solution();
ListNode result = sol.addTwoNumbers(l1, l2);

// To print the list
while (result != null) {
    System.out.print(result.val + " ");
    result = result.next;
}
// Example output: 7 0 8
```

---

## 📂 Project Structure

```
📁 AddTwoNumbers/
 ┣ 📄 ListNode.java
 ┣ 📄 Solution.java
 ┗ 📄 README.md
```

---

## ✅ Notes

- Handles **different lengths** of input lists.
- Automatically includes the **last carry** if needed.
- Uses a **dummy head node** to simplify result list creation.

