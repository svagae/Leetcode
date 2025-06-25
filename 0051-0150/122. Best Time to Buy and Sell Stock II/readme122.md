# Best Time to Buy and Sell Stock II (LeetCode 122)

## Overview

This project provides an efficient Java solution to LeetCode Problem 122, **Best Time to Buy and Sell Stock II**. The goal is to maximize profit by making as many transactions as you like (buy/sell one share multiple times), but you must sell the stock before you can buy again.

---

## Why This Approach?

1. **Greedy Accumulation of Profit**: The problem allows multiple buy/sell transactions, as long as you don't hold more than one stock at a time. This enables a simple greedy approach: whenever the price goes up from one day to the next, we capture that profit. We don't need to time the exact local minima or maxima—just collect all increases.

2. **Linear Time Complexity**: Instead of attempting to find all peaks and valleys, we loop once through the price array and add all positive price differences. This gives us an O(n) time solution, which is optimal given the input size can reach 10⁵.

3. **No Extra Space Needed**: Unlike solutions that simulate real transactions with stacks or dynamic programming arrays, this approach uses only a single integer accumulator. That keeps auxiliary space complexity at O(1).

4. **Mimics Real Trading Behavior**: By treating every increase as a micro-trade (buy yesterday, sell today), we mirror how profits accumulate in an ideal market. We're not aiming to find the "best" trade, but rather to accumulate **all good opportunities**.

---

## Solution Summary

1. **Initialize**:

   * `totalProfit` as 0.

2. **Iterate Through Prices**:

   * For each pair of consecutive days (i and i+1):

     * If `prices[i+1] > prices[i]`, then `prices[i+1] - prices[i]` is a profit opportunity.
     * Add this difference to `totalProfit`.

3. **Return** `totalProfit` after examining all days.

This method ensures that we capture all profitable upward trends in the price array.

---

## Trade-offs and Alternatives

* **Dynamic Programming**: While valid, it is more complex and unnecessary since the greedy approach guarantees optimal profit in this context.
* **Peak-Valley Approach**: You could explicitly find local minima (buy) and maxima (sell), but this ends up being logically equivalent to summing all upward differences—just more verbose.

The greedy method is favored for its clarity, efficiency, and correctness in problems where **multiple sequential transactions are allowed**.

---


