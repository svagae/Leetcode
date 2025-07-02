# 🐢🐇 Floyd's Cycle Detection Algorithm (Tortoise & Hare)

## 📌 Overview

**Floyd’s Cycle Detection Algorithm**, also known as the **Tortoise and Hare Algorithm**, is a classic and efficient technique used to detect **cycles in sequences**—most commonly in **linked lists** or **digit transformations** (like in the Happy Number problem).

It uses two pointers:

* A **slow** pointer (`tortoise`) that moves one step at a time
* A **fast** pointer (`hare`) that moves two steps at a time

If there's a **cycle**, the fast pointer will eventually meet the slow pointer.
If there’s **no cycle**, the fast pointer will reach the end of the sequence.

---

## 📐 Intuition

Imagine two runners on a circular track:

* One runs slowly (1 step per second)
* One runs fast (2 steps per second)

If they both keep running, the faster runner will eventually **lap** the slower runner—proving a **cycle exists**.

If the track is not circular (i.e., no loop), the fast runner will **run off the end**, and no meeting occurs.

---

## 📦 Applications

| Use Case                  | Problem Example                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------- |
| Linked List Cycle         | [LeetCode 141 – Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)       |
| Detect Start of Cycle     | [LeetCode 142 – Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii) |
| Digit transformation loop | [LeetCode 202 – Happy Number](https://leetcode.com/problems/happy-number)                 |
| Functional mappings       | Repeated function states or values                                                        |
| Tortoise & Hare puzzle    | Number sequence problems                                                                  |

---

## 🔧 Basic Algorithm

### Pseudocode

```text
1. Initialize two pointers:
   slow = head
   fast = head

2. Loop while fast and fast.next exist:
   - Move slow by 1 step
   - Move fast by 2 steps
   - If slow == fast → Cycle exists

3. If loop ends → No cycle
```

---

## 🔧 Java Example: Detect Cycle in Linked List

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;

        ListNode slow = head;
        ListNode fast = head.next;

        while (slow != fast) {
            if (fast == null || fast.next == null) return false;
            slow = slow.next;
            fast = fast.next.next;
        }

        return true; // pointers met → cycle
    }
}
```

---

## 🔁 Java Example: Detect Cycle in Number Sequences (Happy Number)

```java
public class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = getNext(n);

        while (fast != 1 && slow != fast) {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        }

        return fast == 1;
    }

    private int getNext(int num) {
        int sum = 0;
        while (num > 0) {
            int d = num % 10;
            sum += d * d;
            num /= 10;
        }
        return sum;
    }
}
```

---

## 📈 Time and Space Complexity

| Metric     | Value                                  |
| ---------- | -------------------------------------- |
| Time       | O(n) or O(log n)                       |
| Space      | O(1)                                   |
| Advantages | No extra memory needed                 |
| Use cases  | Cycles in graphs, lists, number chains |

---

## 🔍 Visual Representation

**With cycle:**

```
         ┌────────────┐
         ↓            │
1 → 2 → 3 → 4 → 5 → 6 ↺
         ↑     ↑
       slow   fast
```

**Without cycle:**

```
1 → 2 → 3 → 4 → 5 → null
        ↑         ↑
      slow      fast
```

---

## 🧠 Key Insights

* Detects whether a **cycle exists**
* Does **not need extra space**
* Works for both **linked structures** and **functional sequences**
* Can be extended to **find the start of the cycle** (with a reset step)

---

## 📘 Further Reading

* [Floyd's Tortoise and Hare Algorithm – Wikipedia](https://en.wikipedia.org/wiki/Cycle_detection)
* [LeetCode Explore – Linked List Cycle](https://leetcode.com/explore/learn/card/linked-list/214/two-pointer-technique/1212/)

---

