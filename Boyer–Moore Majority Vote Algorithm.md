
# 🔍 Boyer–Moore Majority Vote Algorithm – Full Explanation

---

## 🌟 What Is It?

The **Boyer–Moore Majority Vote Algorithm** is a **linear time, constant space algorithm** designed to **find the majority element** in a sequence — that is, an element that appears **more than n / 2 times**, where `n` is the length of the sequence.

> 🔁 You process the input in a single pass and use only two variables.

---

## 🧠 Intuition Behind It

Imagine a voting scenario:

* Every occurrence of an element is like a "vote" for that element.
* Different elements **cancel each other out** like opposing candidates in an election.
* If an element is a true majority (appears more than half the time), **it cannot be fully canceled** — it will always win in the end.

---

## ✅ When To Use It

* You have **a stream of data** and can’t store the whole array.
* You need an algorithm that works in **O(n) time** and **O(1) space**.
* You’re guaranteed (or want to verify) that a majority exists.
* You’re working in constrained environments like:

  * Embedded systems
  * Real-time data processing
  * Election result analysis
  * Signal processing (e.g., finding dominant signals)

---

## 🧩 Step-by-Step Algorithm

### Variables:

* `candidate`: current majority candidate
* `count`: a net count of support for the candidate

### Procedure:

1. **Initialize**:

   ```java
   int candidate = 0;
   int count = 0;
   ```

2. **Loop through the input**:

   ```java
   for (int num : nums) {
       if (count == 0) {
           candidate = num;     // Choose new candidate
       }

       if (num == candidate) {
           count++;             // Same as candidate → increase support
       } else {
           count--;             // Different → decrease support
       }
   }
   ```

3. **Result**:

   * `candidate` is the **potential** majority element.
   * If majority is **not guaranteed**, do a second pass to verify.

---

## 👨‍🏫 Example Walkthrough

Given:

```java
int[] nums = {2, 2, 1, 1, 1, 2, 2};
```

**Step-by-step**:

| Index | num | candidate | count | Explanation                     |
| ----- | --- | --------- | ----- | ------------------------------- |
| 0     | 2   | 2         | 1     | New candidate chosen            |
| 1     | 2   | 2         | 2     | Same as candidate → count++     |
| 2     | 1   | 2         | 1     | Different → count--             |
| 3     | 1   | 2         | 0     | Different → count-- (reaches 0) |
| 4     | 1   | 1         | 1     | New candidate (count was 0)     |
| 5     | 2   | 1         | 0     | Different → count-- (reaches 0) |
| 6     | 2   | 2         | 1     | New candidate                   |

**Final candidate = 2**

---

## ⚠️ Optional Second Pass (Verification)

If there's **no guarantee** a majority exists (e.g., general use cases), **verify**:

```java
int count = 0;
for (int num : nums) {
    if (num == candidate) count++;
}
if (count > nums.length / 2) return candidate;
else return -1; // No majority
```

---

## 📈 Time & Space Complexity

| Metric           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |

---

## 🔄 Generalization (N/K Majority Elements)

You can generalize the algorithm to find **all elements that appear more than n/k times** using the **Misra-Gries algorithm**, which is an extension of Boyer–Moore.

### Basic Idea:

* Maintain up to `k - 1` candidates and their counts.
* For each new element:

  * If it's a current candidate → increase count.
  * Else if there's a free slot → assign it as a new candidate.
  * Else → decrease **all** counts.
* Final pass to verify actual counts.

**Used for:** anomaly detection, frequent item mining, etc.

---

## 🧪 Real-World Use Cases

* **Polling/Voting Systems**: Identify the true majority vote.
* **Telemetry and Logs**: Find the most common event/type.
* **Sensor Data**: Detect dominant signals over time.
* **Natural Language Processing**: Frequent word detection in memory-limited environments.
* **Machine Learning**: Efficient majority label finding during decision tree training.

---

## 🧠 Key Takeaways

* Boyer–Moore is one of the **most elegant algorithms** for linear-time majority detection.
* It’s incredibly **efficient** and easy to implement.
* Works especially well in **streaming or low-memory** situations.
* Can be adapted to **more flexible frequency thresholds**.

---

