 Leetcode 148 – Sort List

### 🔗 Problem Link:

[Leetcode 148 - Sort List](https://leetcode.com/problems/sort-list/)

---

### 🧠 Problem Summary:

You are given the `head` of a **singly linked list**. Sort the list in **ascending order** and return the sorted list.

> ⚠️ You must perform the sort **in-place**, meaning you should not use extra space like arrays or lists.

---

### 📌 Constraints:

* `0 <= Number of nodes <= 5 * 10⁴`
* `-10⁵ <= Node.val <= 10⁵`

---

### 🧪 Example:

#### Input:

```
4 → 2 → 1 → 3
```

#### Output:

```
1 → 2 → 3 → 4
```

---

### 🚫 Brute Force (TLE)

A naive idea is bubble sort with two pointers that swap nodes when values are out of order. But that results in:

* **Time:** O(n²)
* ❌ Fails for large inputs (TLE)

---

### ✅ Optimal Solution – Merge Sort on Linked List

Merge Sort is the best option for linked lists because:

* Linked lists don't support random access (quicksort isn't optimal)
* Merge sort can be done in **O(n log n)** time
* Can be done **in-place**, using recursion and pointer manipulation

---

### 🧱 Approach (Recursive Merge Sort):

#### Step 1: **Split the list**

Use **slow and fast pointers** to find the middle of the list and divide it into two halves.

#### Step 2: **Recursively sort both halves**

#### Step 3: **Merge the sorted halves**

Merge two sorted lists into one sorted list.

---

### ✅ Java Code

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;

        // Step 1: Find the middle
        ListNode slow = head, fast = head, prev = null;
        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        prev.next = null; // split into two halves

        // Step 2: Recursively sort both halves
        ListNode l1 = sortList(head);
        ListNode l2 = sortList(slow);

        // Step 3: Merge the sorted lists
        return merge(l1, l2);
    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }

        tail.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }
}
```

---

### 🔍 Time and Space Complexity

| Complexity | Value                               |
| ---------- | ----------------------------------- |
| ⏱ Time     | O(n log n)                          |
| 🧠 Space   | O(log n) *(due to recursion stack)* |

---

### 📌 Additional Notes:

* `prev.next = null;` is critical to cut the list and prevent infinite loops.
* `merge()` uses a dummy node to simplify pointer logic.
* This version is **recursive**; an iterative bottom-up merge sort version can avoid recursion stack if needed.

---

### 🧠 Dry Run of Merge (for l1 = \[2,4], l2 = \[1,3])

| Step | l1 | l2 | Picked | Result        |
| ---- | -- | -- | ------ | ------------- |
| 1    | 2  | 1  | 1      | 1             |
| 2    | 2  | 3  | 2      | 1 → 2         |
| 3    | 4  | 3  | 3      | 1 → 2 → 3     |
| 4    | 4  | —  | 4      | 1 → 2 → 3 → 4 |

---

### 🔁 We're tracing **this method**:

```java
private ListNode merge(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            tail.next = l1;
            l1 = l1.next;
        } else {
            tail.next = l2;
            l2 = l2.next;
        }
        tail = tail.next;
    }

    tail.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

---

## 🧪 Example:

Let’s merge:

```
l1: 2 → 4
l2: 1 → 3
```

---

## 🎯 GOAL:

Produce:

```
1 → 2 → 3 → 4
```

---

## 🧵 Let's trace:

**Setup:**

* dummy → 0 (just a placeholder)
* tail → dummy
* l1 → 2 → 4
* l2 → 1 → 3

---

### 🔁 1st iteration:

* l1 = 2, l2 = 1
* Compare: 2 vs 1 → 1 is smaller ✅
* `tail.next = l2` → attach 1
* Move `l2` to 3
* Move `tail` to 1

Result so far:

```
dummy → 1
```

---

### 🔁 2nd iteration:

* l1 = 2, l2 = 3
* Compare: 2 vs 3 → 2 is smaller ✅
* `tail.next = l1` → attach 2
* Move `l1` to 4
* Move `tail` to 2

Result so far:

```
dummy → 1 → 2
```

---

### 🔁 3rd iteration:

* l1 = 4, l2 = 3
* Compare: 4 vs 3 → 3 is smaller ✅
* `tail.next = l2` → attach 3
* Move `l2` to null
* Move `tail` to 3

Result so far:

```
dummy → 1 → 2 → 3
```

---

### 🔁 4th iteration:

* l2 is now null
* So we just attach rest of l1 (which is 4)

```java
tail.next = l1; // 4
```

Final merged list:

```
dummy → 1 → 2 → 3 → 4
```

Return:

```java
return dummy.next; // start at 1
```

---

## ✅ Final Output:

```
1 → 2 → 3 → 4
```

---

## 🔄 TL;DR Trace:

| Step | l1 | l2 | Picked | Result        |
| ---- | -- | -- | ------ | ------------- |
| 1    | 2  | 1  | 1      | 1             |
| 2    | 2  | 3  | 2      | 1 → 2         |
| 3    | 4  | 3  | 3      | 1 → 2 → 3     |
| 4    | 4  | —  | 4      | 1 → 2 → 3 → 4 |

---
