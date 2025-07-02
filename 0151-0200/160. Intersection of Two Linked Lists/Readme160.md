# 📌 LeetCode 160 – Intersection of Two Linked Lists

## ✅ Problem Statement

You are given the heads of two singly linked lists: `headA` and `headB`. The two lists may **intersect** at some node, forming a **Y-shaped structure**.

**Goal:** Return the node at which the two lists intersect. If there is **no intersection**, return `null`.

> Note: The intersection is by **reference**, not by value. Two nodes are considered the same if they point to the same memory location, not if they contain the same value.

---

## 🧠 Core Idea: Two-Pointer Trick

We use **two pointers** to traverse both linked lists:

* `pA` starts at the head of List A
* `pB` starts at the head of List B

Whenever a pointer reaches the **end** of its list, it is redirected to the **start of the other list**.

Eventually:

* If the two lists **intersect**, the pointers will meet at the intersection node.
* If they do **not** intersect, both pointers will eventually become `null`.

### Why This Works:

When you switch paths, both pointers end up traversing the same total length:

```
Length A: a + c
Length B: b + c
Total distance each pointer travels: a + b + c
```

So, they will meet at the same node after at most 2 passes.

---

## 🔧 Java Implementation

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;

        ListNode pA = headA;
        ListNode pB = headB;

        // Keep looping until they either meet or both reach null
        while (pA != pB) {
            // Move to the next node or switch to the other list's head
            pA = (pA == null) ? headB : pA.next;
            pB = (pB == null) ? headA : pB.next;
        }

        // Either the intersection node or null
        return pA;
    }
}
```

---

## 🔍 Trace Example

### ✏️ Example Input

```
List A:  1 → 3 → 5 → 7 → 9 → 11
                            ↘
                             13 → 15 → null
                            ↗
List B:        2 → 4 ───────
```

Here, the two lists intersect at node **13**.

### Step-by-Step Traversal

| Step | Pointer A       | Pointer B       |
| ---- | --------------- | --------------- |
| 1    | 1               | 2               |
| 2    | 3               | 4               |
| 3    | 5               | 13              |
| 4    | 7               | 15              |
| 5    | 9               | null            |
| 6    | 11              | 1 (switch to A) |
| 7    | 13              | 3               |
| 8    | 15              | 5               |
| 9    | null            | 7               |
| 10   | 2 (switch to B) | 9               |
| 11   | 4               | 11              |
| 12   | 13 → 🎯 match!  | 13 → 🎯 match!  |

✅ Intersection found at node with value `13`.

---

## 🧮 Time & Space Complexity

| Metric        | Value     |                                            |
| ------------- | --------- | ------------------------------------------ |
| Time          | O(n + m)  | n = length of List A, m = length of List B |
| Space         | O(1)      | No extra space used                        |
| Runtime (avg) | Very fast |                                            |

---

## ❓ Why Not Brute Force?

### Brute Force Approach:

* Compare every node in List A with every node in List B
* Time complexity: **O(n × m)**

### Better Approach Using HashSet:

* Store all nodes of A in a HashSet, then check each node in B
* Time complexity: **O(n + m)** but **Space = O(n)**

### Optimal (Current) Approach:

* Time: **O(n + m)**
* Space: **O(1)**
* No extra memory

---

## ✅ When Will It Return `null`?

If the two lists do **not intersect**, both `pA` and `pB` will eventually reach `null`:

```plaintext
List A: 1 → 2 → 3 → null
List B: 4 → 5 → 6 → null
```

They will never meet and the loop ends with both as `null`, returning `null`.

---

## 🔬 Edge Cases to Consider

* One list is `null` → return `null`
* Both are `null` → return `null`
* Lists intersect at head → return head
* Lists are completely disjoint → return `null`

---

## 🧪 Unit Test Example (Pseudo-Java)

```java
// Create nodes
ListNode c1 = new ListNode(8);
ListNode c2 = new ListNode(10);
c1.next = c2;

ListNode a1 = new ListNode(3);
ListNode a2 = new ListNode(7);
a1.next = a2;
a2.next = c1;

ListNode b1 = new ListNode(99);
ListNode b2 = new ListNode(1);
b1.next = b2;
b2.next = c1;

Solution sol = new Solution();
System.out.println(sol.getIntersectionNode(a1, b1).val); // Output: 8
```

---

## ✅ Summary Table

| Feature                    | Value            |
| -------------------------- | ---------------- |
| Solves intersection check  | ✅                |
| Uses two-pointer trick     | ✅                |
| Requires extra space       | ❌ (O(1))         |
| Supports non-intersecting  | ✅ (returns null) |
| Supports full traceability | ✅                |

---
