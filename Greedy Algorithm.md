# Greedy Algorithms

This repository provides a **comprehensive introduction to Greedy Algorithms**, with clear theoretical explanations and **Java implementations** of classic problems. It is designed for **intermediate to competitive programming (contester) level** learners who want both intuition and practical coding patterns.

---

## 📌 What is a Greedy Algorithm?

A **greedy algorithm** constructs a solution step by step by making the **locally optimal choice at each step**, with the hope that these local choices lead to a **globally optimal solution**.

Once a choice is made, it is **never reconsidered**. This makes greedy algorithms simple, fast, and memory-efficient—but also risky if applied to the wrong problem.

Typical time complexities:

* **O(n)** (simple scans)
* **O(n log n)** (sorting or priority queues)

---

## 🧠 Core Properties

A problem can be solved using a greedy approach **if and only if** it satisfies both of the following:

### 1. Greedy Choice Property

A globally optimal solution can be reached by making a locally optimal choice at each step.

### 2. Optimal Substructure

An optimal solution to the problem contains optimal solutions to its subproblems.

If either property does **not** hold, a greedy approach may fail, and **Dynamic Programming** or **Backtracking** is usually required.

---

## 📚 Classic Greedy Problems (Java Implementations)

### 1️⃣ Activity Selection Problem

**Problem:**
Given start and finish times of activities, select the maximum number of non-overlapping activities.

**Greedy Strategy:**
Always pick the activity that **finishes earliest**.

```java
import java.util.*;

public class ActivitySelection {
    public static void main(String[] args) {
        int[] start = {1, 3, 0, 5, 8, 5};
        int[] finish = {2, 4, 6, 7, 9, 9};

        int n = start.length;
        int[][] activities = new int[n][2];

        for (int i = 0; i < n; i++) {
            activities[i][0] = start[i];
            activities[i][1] = finish[i];
        }

        Arrays.sort(activities, Comparator.comparingInt(a -> a[1]));

        int count = 1;
        int lastEnd = activities[0][1];
        System.out.println("Selected Activities:");
        System.out.println(Arrays.toString(activities[0]));

        for (int i = 1; i < n; i++) {
            if (activities[i][0] >= lastEnd) {
                count++;
                lastEnd = activities[i][1];
                System.out.println(Arrays.toString(activities[i]));
            }
        }
        System.out.println("Total activities selected: " + count);
    }
}
```

---

### 2️⃣ Fractional Knapsack

**Problem:**
Maximize total value in a knapsack where **fractions of items are allowed**.

**Greedy Strategy:**
Sort items by **value / weight ratio** in descending order.

```java
import java.util.*;

public class FractionalKnapsack {
    static class Item implements Comparable<Item> {
        int value, weight;
        double ratio;

        Item(int v, int w) {
            value = v;
            weight = w;
            ratio = (double) v / w;
        }

        public int compareTo(Item other) {
            return Double.compare(other.ratio, this.ratio);
        }
    }

    public static void main(String[] args) {
        int[] values = {60, 100, 120};
        int[] weights = {10, 20, 30};
        int capacity = 50;

        Item[] items = new Item[values.length];
        for (int i = 0; i < values.length; i++)
            items[i] = new Item(values[i], weights[i]);

        Arrays.sort(items);

        double maxValue = 0;
        for (Item item : items) {
            if (capacity == 0) break;

            if (item.weight <= capacity) {
                capacity -= item.weight;
                maxValue += item.value;
            } else {
                maxValue += item.ratio * capacity;
                capacity = 0;
            }
        }
        System.out.println("Maximum value = " + maxValue);
    }
}
```

---

### 3️⃣ Huffman Coding

**Problem:**
Generate an optimal prefix code for lossless data compression.

**Greedy Strategy:**
Repeatedly merge the two nodes with the **lowest frequencies** using a min-heap.

```java
import java.util.*;

public class HuffmanCoding {
    static class Node implements Comparable<Node> {
        char ch;
        int freq;
        Node left, right;

        Node(char c, int f) {
            ch = c;
            freq = f;
        }

        Node(int f, Node l, Node r) {
            ch = '-';
            freq = f;
            left = l;
            right = r;
        }

        public int compareTo(Node o) {
            return this.freq - o.freq;
        }
    }

    static void printCodes(Node root, String code) {
        if (root == null) return;
        if (root.left == null && root.right == null)
            System.out.println(root.ch + ": " + code);
        printCodes(root.left, code + "0");
        printCodes(root.right, code + "1");
    }

    public static void main(String[] args) {
        char[] chars = {'a', 'b', 'c', 'd', 'e', 'f'};
        int[] freq = {5, 9, 12, 13, 16, 45};

        PriorityQueue<Node> pq = new PriorityQueue<>();
        for (int i = 0; i < chars.length; i++)
            pq.add(new Node(chars[i], freq[i]));

        while (pq.size() > 1) {
            Node x = pq.poll();
            Node y = pq.poll();
            pq.add(new Node(x.freq + y.freq, x, y));
        }

        printCodes(pq.poll(), "");
    }
}
```

---

### 4️⃣ Minimum Coin Change (Greedy)

**Problem:**
Find the minimum number of coins needed to make a given amount.

**Greedy Strategy:**
Always use the **largest coin possible**.

```java
public class MinCoins {
    public static void main(String[] args) {
        int[] coins = {25, 10, 5, 2, 1};
        int amount = 63;
        int count = 0;

        for (int coin : coins) {
            while (amount >= coin) {
                amount -= coin;
                count++;
                System.out.print(coin + " ");
            }
        }
        System.out.println("\nTotal coins: " + count);
    }
}
```

⚠️ **Note:** This approach does **not** work for all coin systems.

---

## ❌ When Greedy Fails

Greedy algorithms fail when:

* Local optimal choices do not lead to global optimality
* The problem lacks optimal substructure

Examples where greedy fails:

* **0/1 Knapsack**
* **General Coin Change** (non-canonical systems)
* **Longest Path Problems**
* **Traveling Salesman Problem (TSP)**

In such cases, use **Dynamic Programming**, **Backtracking**, or **Graph Algorithms**.

---

## ▶️ How to Run

```bash
javac FileName.java
java FileName
```

Make sure you have **JDK 8+** installed.

---

## 📈 Competitive Programming Tips

* Always **prove** the greedy choice before implementing
* Try counterexamples
* Greedy + sorting + priority queue is a common pattern
* If greedy fails → try DP

---

## 🔗 Further Practice

* GeeksforGeeks – Greedy Algorithms
* USACO Guide – Greedy
* Codeforces Greedy Tag
* LeetCode Greedy Problems

---

## 📄 License

This repository is open for educational use.

---

**Author:** Mohamed
