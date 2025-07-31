# Backtracking for Combination Problems

Backtracking is a systematic search technique used to build solutions incrementally and **undo** decisions when they lead to dead ends.  It is essentially a depth-first search through the *decision space* of candidate choices: at each step we pick a candidate, recurse, and backtrack by removing the choice when necessary.  This approach is ideal for problems where *all* valid solutions or configurations must be enumerated under constraints (for example, puzzles like Sudoku or problems like N-Queens).

In the *Combination Sum* family of problems (such as LeetCode 39), we are given an array of distinct positive integers (`candidates`) and a target sum, and we must find **all unique combinations** of candidates that add up to the target.  Each candidate can be used **unlimited times**, but the output should not contain duplicate combinations.  Backtracking naturally fits here: we build combinations by repeatedly choosing a candidate and subtracting it from the remaining target, stopping when we hit a valid sum or exceed it.  In practice, we maintain a current combination list and a remaining target value. When the remaining target reaches zero, we record the current combination as a solution.

## Solving Combination Sum with Backtracking

A standard recursive template for combination-sum problems looks like this (in Java):

```java
void backtrack(int[] candidates, int start, int target, 
               List<Integer> comb, List<List<Integer>> results) {
    // Base case: target reached exactly 0
    if (target == 0) {
        results.add(new ArrayList<>(comb));
        return;
    }
    // Try each candidate starting from 'start' index
    for (int i = start; i < candidates.length; i++) {
        // Prune: since array is sorted, no point in continuing if candidate > target
        if (candidates[i] > target) break;
        // Choose the candidate
        comb.add(candidates[i]);
        // Recurse with reduced target; pass 'i' to allow reuse of same element
        backtrack(candidates, i, target - candidates[i], comb, results);
        // Undo choice (backtrack)
        comb.remove(comb.size() - 1);
    }
}
```

Steps in this approach are:

1. **Base Case:** If `target == 0`, the current combination `comb` sums to the original target.  We add a *copy* of `comb` to `results` and return.
2. **For-loop (Choices):** Iterate over candidates from index `start` onward.  This prevents reusing smaller indices and ensures combinations are built in non-decreasing order of indices.
3. **Prune by Sorting:** If we sort `candidates` beforehand, we can break the loop as soon as `candidates[i] > target`.  No further candidates can fit, so we abort this branch (a common pruning strategy).
4. **Choose & Recurse:** If `candidates[i] <= target`, we **add** it to `comb` and recurse with the updated target (`target - candidates[i]`).  We pass `i` again (not `i+1`) to allow the same candidate to be used multiple times.
5. **Backtrack:** After recursion returns, we **remove** the last element from `comb` before the next iteration.  This restores the state for exploring the next choice.

By repeating these steps, the algorithm explores all possible combinations in a DFS manner, but avoids redundant work by pruning impossible paths.

For example, given `candidates = [2,3,6,7]` (sorted) and `target = 7`, the recursion proceeds as follows:

```text
Start: comb = [], target = 7
├─ Try 2 (target ≥ 2): comb=[2], recurse target=5
│   ├─ Try 2 again: comb=[2,2], recurse target=3
│   │   ├─ Try 2 again: comb=[2,2,2], recurse target=1 
│   │   │   • Next candidate is 2 > 1, so branch ends (pruned) [target<0 case].
│   │   ├─ Backtrack to comb=[2,2]  
│   │   ├─ Try 3: comb=[2,2,3], recurse target=0 → valid (record [2,2,3]):contentReference[oaicite:10]{index=10}  
│   │   ├─ Backtrack to comb=[2,2]
│   │   ├─ Next candidates 6,7 are >3 so branch ends.
│   ├─ Backtrack to comb=[2]  
│   ├─ Try 3: comb=[2,3], recurse target=2 
│   │   ├─ Next candidate 3 > 2, skip; 6,7 too big → branch ends.
│   ├─ Backtrack to comb=[2]
│   ├─ Try 6: comb=[2,6], recurse target=-? (<0) prune branch:contentReference[oaicite:11]{index=11} 
│   ├─ Backtrack to comb=[2]
│   ├─ Try 7: comb=[2,7], recurse target=-2 (<0) prune
│   ├─ Backtrack to comb=[]
├─ Backtrack to comb=[]
├─ Try 3: comb=[3], recurse target=4
│   ├─ Try 3: comb=[3,3], recurse target=1 
│   │   ├─ Next candidate 3 > 1, skip; 6,7 >1 → branch ends.
│   ├─ Backtrack to comb=[3]
│   ├─ Try 6: comb=[3,6], recurse target=-? prune
│   ├─ Backtrack to comb=[3]
│   ├─ Try 7: comb=[3,7], recurse target=-? prune
│   ├─ Backtrack to comb=[]
├─ Try 6: comb=[6], recurse target=1 (prune since next candidates >1) 
├─ Backtrack to comb=[]
├─ Try 7: comb=[7], recurse target=0 → valid (record [7]):contentReference[oaicite:12]{index=12}
├─ Backtrack to comb=[]
```

This “recursion tree” (above) illustrates how each branch either finds a valid combination when `target == 0` or is pruned when the remaining target goes negative.  Notice how we never generate duplicate combinations: by always moving forward in the array (`start` index) we ensure each combination is built in sorted order.

## Key Ideas: Pruning and Base Cases

A few crucial insights make backtracking efficient for combination problems:

* **Base Case (target 0 or negative):** If the remaining `target` becomes exactly 0, we have a valid combination and we save it. If `target` drops below 0, no further candidates can fix this (all are positive), so we terminate the current path. Implementing these checks immediately prevents unnecessary exploration.
* **Pruning by Sorting:** Sorting the candidates array is a common optimization. In a loop over sorted candidates, as soon as `candidates[i] > target`, we can `break` out of the loop and backtrack.  This cuts off many branches early.
* **Reuse vs. Move Forward:** For problems like Combination Sum where elements *can* be reused, we recurse with the same index `i` to allow picking the same element again.  If elements could only be used once, we would recurse with `i+1` instead.  This index strategy also ensures combinations are generated in non-decreasing order, avoiding duplicates.
* **Backtracking Step:** After each recursive call, we **remove** the last added element (undo the choice).  This backtracking is what restores state for the next iteration. For example, after exploring `[2,2,3]`, we remove the `3` and try the next possibility in that branch.
* **Avoiding Redundancy:** By enforcing an order (using the `start` index) and pruning invalid paths early, backtracking avoids building permutations of the same combination.  In practice, this means we never revisit previously considered choices at the same recursion depth. The combination `[2,3]` will only appear in one order (`[2,3]`, not `[3,2]`) because we only add elements in forward index order.

As one guide summarizes: “The base case for the recursion is if the target is 0. When the target is 0, we append a copy of the current combination to the result list. Then \[we] iterate through the candidates starting from the given index. If the candidate is greater than the remaining target, we skip it; if it is smaller, we add it, recurse, and then backtrack by removing it from the combination”. This pattern embodies the core backtracking logic for combination-sum problems.

## Best Practices and Optimization

To write effective backtracking solutions for combinations, keep these tips in mind:

* **Pre-sort Candidates:** Always sort the input array first.  It enables the `if (candidate > target) break;` pruning inside the loop, which can dramatically cut down recursive calls.
* **Use a Helper Function:** Factor the recursive backtracking into a helper (as shown) so that you can pass the current combination and index. This keeps the code clean and focused.
* **Copy on Save:** When `target == 0`, add a *copy* of the current combination to the results (e.g. `new ArrayList<>(comb)`).  Do not add the reference directly, since the list will be modified by backtracking.
* **Careful with Indices:** Use the `start` index to avoid reuse of previous elements.  Passing `i` into the next call allows repeating the same element; passing `i+1` would forbid reuse (useful if each element can only be used once).  This index trick also implicitly avoids generating the same combination multiple ways.
* **Prune Early:** In addition to sorting, include an explicit check `if (target < 0) return;` in your function.  This terminates branches as soon as they become invalid.
* **Handle Duplicates (if needed):** If the input array can contain duplicates and you need unique combinations (e.g. Combination Sum II), you can skip over equal adjacent candidates in the loop (`if (i > start && candidates[i] == candidates[i-1]) continue;`).  In the classic Combination Sum I (distinct input), this isn’t needed, but it’s a common technique for similar problems.
* **Limit Scope of Change:** Inside the loop, only modify the combination list (`comb.add(...)` and `.remove(...)`) and the remaining target. Do not modify the original array or global state. After backtracking, the state should be exactly as it was before the choice.
* **Iterative vs Recursive:** Although backtracking is inherently recursive, you can think of it as a DFS through an implicit tree of decisions. Writing it recursively is simplest, but if recursion depth is a concern, an explicit stack can simulate the same process

By following this pattern and using these optimizations, backtracking solutions become both clear and efficient. They explore the search space in a disciplined way, pruning impossible branches and avoiding duplicate work. In competitive programming, this means you can handle reasonably large inputs (subject to exponential complexity) with clear, concise code.

Overall, backtracking for combination problems is about **systematic exploration**: at each step, choose a candidate, recurse, and undo.  The combination-sum example shows the pattern vividly: choose numbers that reduce the remaining target, stop when you hit zero, and cut off branches that exceed the target.  With careful base-case checks and pruning, this brute-force search is made practicable and clear to implement for contest and interview problems.

**References:** Key points above are drawn from algorithmic tutorials on backtracking and from in-depth explanations of the Combination Sum pattern. These sources highlight the recursion structure, base cases, and pruning strategies that make backtracking work efficiently
