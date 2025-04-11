#  Two Sum Solution

## 📝 Overview
This repository contains a solution to the classic **Two Sum** problem, commonly found in coding interviews and algorithm challenges.

- **Language**: Java  
- **Problem Statement**:  
  Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that their sum equals the target.

### 🔒 Constraints
- Exactly **one** solution exists.
- Each input can be used **once**.
- Cannot use the **same element twice**.

---

## 💻 Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int l = nums.length;
        int[] res = new int[2]; 
        
        for (int i = 0; i < l; i++) {
            for (int j = i + 1; j < l; j++) {
                if (nums[i] + nums[j] == target) {
                    res[0] = i;
                    res[1] = j;
                    return res;
                }
            }
        }
        
        return null; 
    }
}
```

---

## 📖 Explanation

### 🔢 Input
- `nums`: An array of integers. Example: `[2, 7, 11, 15]`
- `target`: An integer representing the target sum. Example: `9`

### 🎯 Output
- An array of two integers `[i, j]` where `nums[i] + nums[j] == target`.

### 🧠 Logic
The solution uses a **brute force** approach with nested loops to find a valid pair of numbers.

#### 🔄 Steps:
1. **Initialize**:
   - `l = nums.length` stores the array's length.
   - `res = new int[2]` to hold the result indices.

2. **Loop through all pairs**:
   - Outer loop runs from `0` to `l-1`.
   - Inner loop starts from `i + 1` to avoid:
     - Using the same element twice.
     - Re-checking pairs (i, j) and (j, i).

3. **Check Condition**:
   - If `nums[i] + nums[j] == target`, store indices and return.

4. **No Match Case**:
   - Returns `null` only if no solution is found (theoretically unreachable).

---

## 📌 Example

### ✅ Input
```java
nums = [2, 7, 11, 15]
target = 9
```

### 🧪 Execution
- Loop starts with `i = 0`, `j = 1`
- `nums[0] + nums[1] = 2 + 7 = 9`
- Matches target → returns `[0, 1]`

### 🧾 Output
```java
[0, 1]
```

---

## ⏱️ Time & Space Complexity

| Metric           | Complexity |
|------------------|------------|
| **Time**         | O(n²)      |
| **Space**        | O(1)       |

---

## 🛠️ Notes
- The brute force solution is simple but inefficient for large inputs.
- Optimized solutions using hash maps can achieve **O(n)** time.
- This code avoids using the same index twice by starting the inner loop from `j = i + 1`.

---

## 🚀 Usage

```java
Solution solution = new Solution();
int[] nums = {2, 7, 11, 15};
int target = 9;
int[] result = solution.twoSum(nums, target);
System.out.println(Arrays.toString(result)); // Output: [0, 1]
```

> 💡 Make sure to `import java.util.Arrays;` if you use `Arrays.toString()`.

---

## 📂 Project Structure

```
📁 TwoSum/
 ┣ 📄 Solution.java
 ┗ 📄 README.md
```
