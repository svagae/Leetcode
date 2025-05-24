# Remove Duplicates from Sorted List

## Problem Description

Given the head of a **sorted** linked list, delete all duplicates such that each element appears only once. Return the linked list **sorted** as well.

> This is LeetCode Problem [83. Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

### Example

```
Input:  head = [1,1,2]
Output: [1,2]

Input:  head = [1,1,2,3,3]
Output: [1,2,3]
```

---

## Approach

This solution leverages the **sorted property** of the linked list to efficiently remove duplicates in a single pass.

### Strategy

* Initialize a pointer `current` at the `head` of the list.
* Traverse the list using a `while` loop until `current` or `current.next` is null.
* For each node:

  * If the value of the current node is equal to the next node, we skip the next node by pointing `current.next` to `current.next.next`.
  * Otherwise, move the `current` pointer one step forward.
* This way, all duplicate elements are removed in place without using extra space.

### Time & Space Complexity

* **Time Complexity:** O(n), where n is the number of nodes in the linked list.
* **Space Complexity:** O(1), as no extra space is used.

---

## Code

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode current = head;
        while (current != null && current.next != null) {
            if (current.val == current.next.val) {
                current.next = current.next.next;
            } else {
                current = current.next;
            }
        }
        return head;
    }
}
```


