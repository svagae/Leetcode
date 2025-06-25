# Best Time to Buy and Sell Stock (LeetCode 121)

## Overview

This project provides an efficient Java solution to LeetCode Problem 121, **Best Time to Buy and Sell Stock**. The goal is to maximize profit by choosing one day to buy and a later day to sell a single share of stock.

---

## Why This Approach?

1. **Optimized Time Complexity**: The naive brute-force method checks every possible pair of buy and sell days, resulting in O(n²) time. With large input sizes (up to 10⁵ days), this approach becomes impractical. Instead, our solution scans the price list once, achieving O(n) time complexity.

2. **Constant Space Usage**: Some dynamic programming solutions or additional data structures (e.g., segment trees) might solve the problem but consume extra memory. By keeping only two integer variables—one for the minimum price seen so far and one for the maximum profit—we maintain O(1) auxiliary space.

3. **Greedy Intuition**: This problem fits a greedy strategy:

   * Always consider the **lowest price encountered** as the best potential buy-day.
   * On each subsequent day, calculate the profit if selling today and compare it to the best profit so far.
   * Update the minimum purchase price only when you see a cheaper opportunity.

   This greedy perspective ensures we never miss an optimal transaction because any higher selling price after the global minimum yields the best possible profit.

4. **Simplicity and Readability**: By abstracting away unnecessary structures and focusing on two core concepts—min price and profit—the solution remains concise and easy to understand, reducing the risk of bugs.

---

## Solution Summary

1. **Initialize**:

   * `minPrice` to a very large value (to ensure the first price becomes the new minimum).
   * `maxProfit` to zero.

2. **Single Pass Scan**:

   * For each stock price:

     * If the current price is lower than `minPrice`, update `minPrice` (found a better buy-day).
     * Otherwise, compute potential profit (`currentPrice - minPrice`) and update `maxProfit` if this profit exceeds the previous best.

3. **Return** the `maxProfit` after traversing all prices.

---

## Trade-offs and Alternatives

* **Dynamic Programming**: Could formalize this as a DP problem, but that often involves more state tracking and extra arrays.
* **Divide & Conquer or Segment Trees**: Overkill for a single-pass linear problem.
* **Kadane’s Algorithm Analogy**: The profit array (differences between consecutive days) can map to a maximum subarray sum problem. While insightful, it adds unnecessary transformation for this simple buy-sell scenario.

The chosen greedy one-pass solution strikes the best balance between performance, simplicity, and memory efficiency.

---
