# 🧪 LeetCode 234 – Palindrome Linked List

## ✅ Problem Statement

Given the head of a singly linked list, determine whether it is a **palindrome**.

A palindrome is a sequence that reads the same backward as forward.

---

## 🔍 Examples

### Example 1

```
Input: 1 → 2 → 2 → 1
Output: true
```

### Example 2

```
Input: 1 → 2
Output: false
```

---

## 🧠 Optimal Approach: Reverse Second Half and Compare

### Idea

We can’t move backward in a singly linked list, so we do the next best thing:

1. Use the **two-pointer technique** (fast & slow) to find the middle of the list.
2. **Reverse the second half** of the list.
3. Compare the **first half** and **reversed second half** node by node.
4. (Optional) **Restore** the list structure by reversing again.

---

## 🔧 Java Implementation

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;

        // Step 1: Find the middle
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Step 2: Reverse second half
        ListNode secondHalfStart = reverseList(slow);

        // Step 3: Compare both halves
        ListNode firstHalf = head;
        ListNode secondHalf = secondHalfStart;
        while (secondHalf != null) {
            if (firstHalf.val != secondHalf.val) {
                return false;
            }
            firstHalf = firstHalf.next;
            secondHalf = secondHalf.next;
        }

        // Optional Step 4: Restore the original list (optional)
        // reverseList(secondHalfStart);

        return true;
    }

    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode nextTemp = head.next;
            head.next = prev;
            prev = head;
            head = nextTemp;
        }
        return prev;
    }
}
```

---

## 📊 Time and Space Complexity

| Metric           | Value  |
| ---------------- | ------ |
| Time Complexity  | O(n)   |
| Space Complexity | O(1) ✅ |

* Reversing the second half is in-place, so no extra memory is used.
* Only one pass is needed to compare.

---

## 🧪 Visual Example

Given:

```
1 → 2 → 3 → 2 → 1
```

* After finding the middle:

  * `slow` points to `3`
* Reverse second half:

  * `2 → 1` becomes `1 → 2`
* Compare:

  * `1 == 1`
  * `2 == 2`
  * Done ✅

---

## 🧠 Key Insights

* Don’t try to walk backwards; instead, reverse the second half.
* Don’t use extra structures like arrays or stacks if unnecessary.
* This method ensures **constant space** and is perfect for **interviews** or **production-safe checks**.

---

## 📘 Related Problems

* [LeetCode 206 – Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
* [LeetCode 21 – Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
* [LeetCode 141 – Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

---

## ✅ Summary

| Feature        | This Solution |
| -------------- | ------------- |
| In-place       | ✅             |
| Constant space | ✅             |
| Optimal time   | ✅ O(n)        |
| List restored  | Optional      |

---

