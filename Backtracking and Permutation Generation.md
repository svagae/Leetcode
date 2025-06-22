


# Backtracking and Permutation Generation

Backtracking is an algorithmic technique that incrementally builds candidates to solutions and abandons (“backtracks”) them if they violate constraints.  In other words, it explores all possibilities in a depth-first manner, undoing the last decision when no solution can be completed from the current partial state.  This approach is well-suited to combinatorial problems like generating **permutations** of a set, where we need to consider all orderings of the input elements. For example, 3 distinct items have 3! = 6 permutations, and in general `n` distinct items have `n!` permutations (factorial growth).  Backtracking can systematically generate each arrangement and **reject** partial sequences once they cannot lead to a new permutation.

## Generating Permutations by Recursion

A backtracking solution for permutations works by building each permutation one element at a time and using recursion to explore all choices at each step. The key idea is to place elements in positions **recursively** and swap or mark them as used. A general outline is:

1. **Base case**: If we have placed all elements (the current partial permutation has length `n` for an `n`-element input), then we have a complete permutation. We record or output it.
2. **Recursive step**: Otherwise, at position `idx` (starting from 0), we try each available element for that position, then recurse for the next position. After returning from recursion, we **backtrack** (undo the choice) to restore the state for the next iteration.
3. **Avoid repeats**: If the input has duplicate values, we typically sort the array first and *skip* choices that would duplicate a permutation. Specifically, if `arr[i]` is the same as `arr[i-1]` and `arr[i-1]` has not been used in the current branch, we skip `arr[i]`. This ensures unique permutations.

In summary, the backtracking process builds a partial “path” (the current permutation) and at each recursive call, it chooses an unused element and recurses deeper. Once a full permutation is built, it is recorded. Then the algorithm backtracks by removing the last element and tries the next possibility. This systematically covers all permutations without revisiting the same state.

## Recursive Structure and Base Case

The **recursive structure** of the permutation generator has a clear base case and recursion:

* **Base case:** When the recursion depth reaches the size of the input (e.g. `idx == n`), all positions are filled and a valid permutation has been formed. The algorithm then typically **adds a copy** of the current arrangement to the result list and returns. For example, in code, this is often:

  ```java
  if (idx == arr.length) {
      result.add(convertArrayToList(arr));  // a complete permutation
      return;
  }
  ```

  This termination condition stops further recursion and begins the unwinding (backtracking) process.

* **Recursive step:** If we are not at the base case, we perform a loop over candidate elements to place at the current position. In the simple “swap” approach, we swap each element at index `i` with the current index and recurse, as shown below. In the “visited” approach (building a list), we loop over all elements and skip those already used. In either case, after the recursive call, we undo the last choice (swap back or mark unused) to restore the state.

These two parts ensure that the recursion tree explores all permutations: every branch picks one element and goes deeper until completion, then backtracks to try another element.

## Steps of the Backtracking Algorithm

A concise breakdown of the backtracking algorithm for permutations is:

1. **(Optionally) Sort the input array** if duplicates must be handled. Sorting groups identical values, which is used later to skip duplicates.
2. **Use a `visited[]` array (or swap approach)** to keep track of which elements are in the current partial permutation.
3. **Iterate** over all indices `i`:

   * If element `i` is already used, skip it.
   * If handling duplicates: if `arr[i] == arr[i-1]` and `arr[i-1]` **is not used** in the current permutation, skip `arr[i]` to avoid repeating a permutation.
   * **Choose** element `arr[i]` for the next position (mark it used or swap it into place).
   * **Recurse** to fill the next position (increment the position or recursion depth).
   * **Backtrack**: undo the choice (remove from current path or swap back, and mark the element unused).
4. **Record a result** whenever the current permutation reaches full length.

This step-by-step process is outlined in many algorithm tutorials. By following these steps, the algorithm exhaustively builds all possible orderings.

## Java Implementation: Permutations Without Duplicates

Below is a Java example that generates all permutations of an array of *distinct* integers using backtracking by swapping in-place. The code uses recursion and a helper `swap` function. Comments explain the base case and recursion:

```java
import java.util.*;

public class Permutations {
    // Helper to swap elements in the array
    private static void swap(int[] arr, int i, int j) {
        int tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
    }

    // Recursive function to generate permutations by swapping
    static void permute(int[] arr, int idx, List<List<Integer>> result) {
        // Base case: if idx reaches end, record the permutation
        if (idx == arr.length) {
            // Convert array to list and add to results
            List<Integer> perm = new ArrayList<>();
            for (int num : arr) perm.add(num);
            result.add(perm);
            return;
        }
        // Try each element for position idx
        for (int i = idx; i < arr.length; i++) {
            swap(arr, idx, i);                  // Place arr[i] at index idx
            permute(arr, idx + 1, result);     // Recurse for next position
            swap(arr, idx, i);                  // Backtrack: undo swap
        }
    }

    // Public method to initiate permutation generation
    public static List<List<Integer>> permute(int[] arr) {
        List<List<Integer>> result = new ArrayList<>();
        permute(arr, 0, result);
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        List<List<Integer>> perms = permute(arr);
        for (List<Integer> perm : perms) {
            System.out.println(perm);
        }
    }
}
```

In this code:

* The **base case** `if (idx == arr.length)` detects a complete permutation and adds it to the result list.
* The **loop** from `i = idx` to end swaps each possible element into the `idx` position, then recurses to fill the rest.
* After recursion, we **swap back** to restore the original array before trying the next choice (backtracking). This ensures that the array is unchanged when the function returns.
* This method will output all `n!` permutations (for distinct elements), e.g. for `{1,2,3}` it prints `[1,2,3]`, `[1,3,2]`, `[2,1,3]`, `[2,3,1]`, `[3,1,2]`, `[3,2,1]`.

## Handling Duplicate Elements (Unique Permutations)

If the input array may contain **duplicate values**, a naive permutation generator will produce repeated lists. To generate **only distinct permutations**, a common variation is:

* **Sort the array first**, so equal elements are adjacent.
* Use a boolean `visited[]` array (same length as `arr`) to mark which indices have been used in the current permutation.
* In the recursion, **skip duplicates**: if `i>0` and `arr[i] == arr[i-1]` *and* `visited[i-1]` is `false`, then continue to next `i`. This ensures that among equal values, we only use the first unused one in each position.

These rules ensure each unique arrangement is generated exactly once. The algorithm steps for the duplicate-aware version are:

1. Sort the array (so duplicates are adjacent).
2. Recurse from position 0, using a boolean `visited[]`.
3. For each index `i`:

   * If `visited[i]` is true, skip it.
   * If `i>0 && arr[i] == arr[i-1] && !visited[i-1]`, skip it (avoid duplicate).
   * Otherwise, mark `visited[i]=true` and add `arr[i]` to the current path.
   * Recurse to fill the next position.
   * Backtrack: remove `arr[i]` from path and set `visited[i]=false`.
4. When the path length equals `arr.length`, record a copy of the path as a complete permutation.

A Java implementation of this approach (unique permutations with duplicates) is shown below. This uses an `ArrayList<Integer> curr` to build each permutation and a list of lists for results:

```java
import java.util.*;

public class UniquePermutations {
    // Backtracking function to generate unique permutations
    static void backtrack(int[] arr, boolean[] visited, 
                          List<Integer> curr, List<List<Integer>> result) {
        // Base case: complete permutation built
        if (curr.size() == arr.length) {
            result.add(new ArrayList<>(curr));  // add a copy of current permutation
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            // Skip already visited elements
            if (visited[i]) continue;
            // Skip duplicates: if arr[i] == arr[i-1] and previous not used
            if (i > 0 && arr[i] == arr[i-1] && !visited[i-1]) continue;
            // Choose arr[i] for current position
            visited[i] = true;
            curr.add(arr[i]);
            // Recurse for next position
            backtrack(arr, visited, curr, result);
            // Backtrack: remove last element and mark as unused
            curr.remove(curr.size() - 1);
            visited[i] = false;
        }
    }

    // Public method to get all unique permutations
    public static List<List<Integer>> permuteUnique(int[] arr) {
        Arrays.sort(arr);  // Sort to handle duplicates
        List<List<Integer>> result = new ArrayList<>();
        backtrack(arr, new boolean[arr.length], new ArrayList<>(), result);
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 3};
        List<List<Integer>> perms = permuteUnique(arr);
        for (List<Integer> perm : perms) {
            System.out.println(perm);
        }
    }
}
```

**Explanation of key parts:**

* We sort the array at the start (`Arrays.sort(arr)`) so that equal values are adjacent.
* The `visited[]` array tracks which indices are already used in the current permutation. This prevents reusing an element.
* The duplicate-skip condition `if (i>0 && arr[i]==arr[i-1] && !visited[i-1]) continue;` ensures that for equal elements, we only take the later one if the previous identical element has already been used in this permutation. This avoids generating the same arrangement in a different order.
* When `curr.size() == arr.length`, we have a full unique permutation to record. We add a *copy* of `curr` to results.
* After the recursive call, we remove the last element and unmark it (`visited[i]=false`) to explore other possibilities (backtracking).

For the sample input `{1, 3, 3}`, this code will output the distinct permutations:

```
[1, 3, 3]
[3, 1, 3]
[3, 3, 1]
```

Each permutation is unique and the duplicate `[3,3,1]` only appears once.

## Time and Space Complexity

Generating all permutations is inherently expensive because the number of permutations is factorial in `n`. In fact, the time complexity of a backtracking permutation generator is **O(n · n!)** in the worst case. There are `n!` permutations and it takes O(n) to copy or output each one, giving O(n · n!) overall. The space complexity (ignoring the output size) is O(n) due to the recursion stack and the `curr` list, but storing the results itself requires O(n · n!) space in the output list.

In practice, this means even moderate input sizes (n > 10) can produce very large numbers of permutations. For example, 10 distinct items have 10! = 3,628,800 permutations. Thus, permutation generation is typically only feasible for small `n`, and handling duplicates (by pruning equivalent branches) can significantly reduce the output size.

## Summary

Backtracking is a powerful recursive technique for generating all permutations of a set. The algorithm builds permutations one element at a time, using a base case when a full permutation is formed, and backtracking (undoing the last choice) to explore other branches. In Java, this is often implemented either by *swapping* elements in-place or by using a boolean `visited[]` array to build a current list of elements. When duplicates are present, sorting the input and skipping over repeated values ensures unique results. Code comments and careful base-case definition make the recursive structure clear. This approach comprehensively generates each valid permutation (in about O(n · n!) time) while illustrating core recursion and backtracking concepts for students and competitive programmers.

**Sources:** Algorithm tutorials and references on backtracking and permutations. These explain the incremental construction of solutions, the base case at full length, and the standard duplicate-handling trick of sorting and skipping.
