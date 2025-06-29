# Binary Search: Fundamentals and Techniques

Binary search is a classic *divide-and-conquer* algorithm for finding an element in a **sorted** array or list.  At each step, binary search compares the target value to the middle element of the current search interval and halves the search space accordingly.  If the middle element equals the target, the search ends.  Otherwise, if the target is smaller, the algorithm continues in the left half; if larger, in the right half.  Because the data is sorted, each comparison eliminates half of the remaining elements, yielding a time complexity of *O(log n)*.  GeeksforGeeks notes that this “reduces the time complexity to O(log N)”.  Iterative binary search uses only constant extra space (O(1) space), while a recursive version uses O(log n) stack space in the worst case.  In practice, binary search requires **random access** to elements (constant-time access), so it is typically applied to arrays or array-backed lists.

Key conditions for binary search are that the data must be sorted and elements are indexable.  The algorithm can be summarized in steps:

* **Step 1:** Initialize `low = 0` and `high = n-1` for an array of size `n`.
* **Step 2:** While `low <= high`, compute `mid = low + (high - low)/2` (using a safe midpoint calculation to avoid overflow).
* **Step 3:** If `array[mid] == target`, return `mid`.
* **Step 4:** If `array[mid] < target`, set `low = mid + 1` (search right half); else set `high = mid - 1` (search left half).
* **Step 5:** If the loop ends without finding the target, the element is not present.

This basic logic is the foundation for both iterative and recursive implementations, and for various binary-search-based patterns.  By halving the search space each iteration, binary search achieves logarithmic time complexity.

## Iterative Binary Search in Java

The **iterative** form of binary search uses a loop to narrow the search interval.  At each iteration, it calculates a middle index and adjusts either the left (`low`) or right (`high`) bound until the target is found or the interval is empty.  One common pitfall is integer overflow when computing `mid = (low + high) / 2`. A safer calculation is `mid = low + (high - low)/2`, which avoids overflow.

Here is a Java implementation of iterative binary search:

```java
public class BinarySearchIterative {
    // Returns index of target in arr, or -1 if not found
    public static int binarySearch(int[] arr, int target) {
        int low = 0, high = arr.length - 1;
        while (low <= high) {
            // Safe midpoint calculation to avoid overflow
            int mid = low + (high - low) / 2;
            if (arr[mid] == target) {
                return mid;               // target found
            }
            if (arr[mid] < target) {
                low = mid + 1;           // search right half
            } else {
                high = mid - 1;          // search left half
            }
        }
        return -1; // target not found
    }

    public static void main(String[] args) {
        int[] data = {2, 3, 4, 10, 40};
        int target = 10;
        int result = binarySearch(data, target);
        System.out.println(
            (result == -1) ? "Not found" : "Found at index " + result
        );
    }
}
```

In this code, `binarySearch` repeatedly halves the interval `[low..high]` until `low > high` or the element is found.  The space usage is constant (apart from the input array) because we use only a few variables.  GeeksforGeeks similarly shows this approach and notes it runs in O(log n) time.

## Recursive Binary Search in Java

The **recursive** version applies the same logic but uses function calls instead of a loop.  The array bounds are passed as parameters in each call.  The base case checks if `low > high` (target not found) or if `arr[mid]` is the target.  Otherwise, the function recurses into either the left or right half.  The code below uses a helper method that takes the current low and high indices:

```java
public class BinarySearchRecursive {
    // Wrapper method
    public static int binarySearch(int[] arr, int target) {
        return binarySearch(arr, 0, arr.length - 1, target);
    }

    // Recursive binary search between indices low and high
    private static int binarySearch(int[] arr, int low, int high, int target) {
        if (low > high) {
            return -1; // target not in array
        }
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) {
            return mid;   // target found
        }
        if (arr[mid] > target) {
            return binarySearch(arr, low, mid - 1, target);  // search left half
        } else {
            return binarySearch(arr, mid + 1, high, target); // search right half
        }
    }

    public static void main(String[] args) {
        int[] data = {2, 3, 4, 10, 40};
        int target = 10;
        int result = binarySearch(data, target);
        System.out.println(
            (result == -1) ? "Not found" : "Found at index " + result
        );
    }
}
```

This recursive method calls itself with a smaller interval on each step.  The time complexity remains O(log n), but the recursive calls use O(log n) stack space in the worst case (due to recursion depth) – whereas the iterative version used only O(1) extra space.  In practice, modern runtimes optimize tail recursion poorly in Java, so the iterative version is usually preferred to avoid recursion overhead.

## Iterative vs. Recursive: Comparison and Pitfalls

Both iterative and recursive binary search implement the same logic, but they have trade-offs:

* **Space:** The iterative version uses constant extra space (just a few variables) while the recursive version uses stack space proportional to the recursion depth (O(log n)).  In languages like Java, deep recursion can risk stack overflow for very large arrays.
* **Readability:** Recursive code can be more concise, but iterative code is often easier to reason about in interviews (fewer edge-case mistakes).
* **Performance:** Iteration generally has a tiny constant advantage (no function call overhead).  The time complexity is identical (O(log n)) either way.
* **Midpoint calculation:** In both methods, calculate midpoint as `low + (high - low) / 2` to avoid integer overflow.  Forgetting this can cause errors on large indexes.
* **Loop/recursion conditions:** A common pitfall is an infinite loop or missing the target.  For example, using `while (low < high)` without correctly adjusting or checking bounds can skip elements.  Always ensure that the termination condition (`low > high`) correctly indicates an empty interval.
* **Edge cases:** When the target is not present, make sure to return a default (e.g. -1 or false) after the search ends.  Similarly, handle empty arrays (simply return -1/false immediately).
* **Off-by-one errors:** If using  `while (low < high)` style loops, one must be careful with updating `low` and `high` (sometimes using `(low+high+1)/2` as mid) to avoid infinite loops. The USACO guide notes that certain mid calculations with `<` can loop infinitely for intervals of size one. A safer pattern is `while (low <= high)` with the standard mid formula.

In coding interviews, the iterative form is often favored for clarity and avoiding recursion limits, but knowing both is useful for understanding different problems.

## Finding First/Last Occurrences (Lower/Upper Bound Variants)

Standard binary search finds *an* index of the target, but in arrays with **duplicates**, we often need the first or last occurrence (or boundaries).  These are classic variations:

* **First occurrence:** Find the smallest index where `arr[index] == target`.  We modify binary search so that when we find the target, we continue searching to the left (lower indices) to see if an earlier occurrence exists.
* **Last occurrence:** Find the largest index with `arr[index] == target`.  Here, when we find the target, we search to the right (higher indices).
* **Lower bound:** Find the first index where `arr[index] >= target`. If no element is exactly `target`, this gives where it *would* be inserted.
* **Upper bound:** Find the first index where `arr[index] > target`.

All of these can be implemented by tweaking the binary search loop.  For example, to find the **first occurrence**, one can do:

```java
public static int findFirst(int[] arr, int target) {
    int low = 0, high = arr.length - 1;
    int result = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) {
            result = mid;      // potential first occurrence
            high = mid - 1;    // search left half for earlier occurrence
        } else if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return result;
}
```

Similarly, for the **last occurrence**, we search right when we see the target:

```java
public static int findLast(int[] arr, int target) {
    int low = 0, high = arr.length - 1;
    int result = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) {
            result = mid;
            low = mid + 1;     // search right half for a later occurrence
        } else if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return result;
}
```

If the target does not exist, both methods return -1.  These approaches match the description in GeeksforGeeks: “find the first and last occurrence of a given number separately using binary search”.  Conceptually, this uses the same O(log n) logic but continues searching after finding the target.

The **lower bound** can be implemented by treating `arr[mid] >= target` as a “match” and moving left on match, similar to first occurrence; the **upper bound** uses `arr[mid] > target` as the condition to move left.  In many languages (e.g. C++), standard libraries expose lower-bound/upper-bound routines, but in interviews you often write these manually.  The key idea is that by adjusting the branch conditions (≤ or < and whether to update `low` or `high`), you can locate the boundary indices.

## Real-World Use Cases of Binary Search

Binary search underlies many practical computing tasks whenever data is sorted or when searching monotonic domains.  Some common examples include:

* **Searching in sorted arrays or lists:** The most direct use is finding an element in a sorted list of items (numbers, strings, etc.).  For example, looking up a word in a sorted dictionary or product in a sorted inventory.  Many libraries exploit this – e.g. Java’s `Arrays.binarySearch()` method searches a sorted array in O(log n) time (the array must be sorted first).  Likewise, sorted containers like `TreeSet` or `TreeMap` in Java are conceptually based on binary-search-like operations.
* **Database indexing:** Databases often index data using B-trees or B+ trees, which are balanced tree structures generalizing binary search trees.  A B-tree node allows multi-way branching, but the search within each node is a form of binary (or linear) search over the node’s sorted keys.  As Youcademy notes, binary search in indexes “avoids full table scans” and reduces lookup time from O(n) to O(log n).  Similarly, range queries (e.g. find all entries between two values) use binary search on the sorted index to find the start and end positions efficiently.
* **Version control (Git bisect):** Tools like `git bisect` use binary search on the commit history to find which commit introduced a bug.  By successively testing a commit (good or bad) and eliminating half of the remaining range, the algorithm pinpoints the offending change in O(log N) steps.  This is literally a binary search in the space of commits.
* **Numeric and optimization problems:**  When dealing with a monotonic function (one that always increases or decreases), binary search can find threshold points.  For instance, in graphics or scientific computing, one might binary-search a parameter value to satisfy a condition.  In computational math, binary search is used to find square roots or to solve equations by narrowing intervals.
* **System tools (logs, scheduling):** System administrators searching a timestamped log file can use binary search to find entries near a given time, rather than scanning sequentially.  Rate limiting and load balancing algorithms sometimes use binary search over integer parameters to tune thresholds.
* **Miscellaneous:** Any time a solution space is sorted or can be made monotonic, binary search can apply.  For example, finding the first time a program fails (test condition becomes true) or optimizing a value for which a predicate is true can use a binary-search pattern.

These cases highlight that binary search is not limited to simple arrays; it is a fundamental tool in databases, version control, scheduling, and more (e.g. Git’s bisect “eliminating half the possibilities”).  Its power comes from drastically reducing search time on large datasets, which is why it frequently appears in coding interviews and system design.

## Binary Search in 2D: The Flattened Matrix Pattern

A common interview variant is searching a **2D matrix** with sorted properties.  For example, LeetCode Problem 74 (“Search a 2D Matrix”) defines a matrix where each row is sorted in ascending order, and the first integer of each row is greater than the last integer of the previous row.  This means the entire matrix behaves like one big sorted list (row-major order).  In other words, you can conceptually “flatten” the matrix and apply binary search to the single-dimensional index.

Formally, if the matrix has `m` rows and `n` columns, we can treat an index `idx` (from `0` to `m*n-1`) as mapping to row and column by:

```
row = idx / n;
col = idx % n;
```

Likewise, a matrix element at `[row][col]` can be seen at the flattened index `row * n + col`.  For example, the element `matrix[i][j]` would be at index `i*n + j` in the virtual array. The AlgoMonster guide explains this mapping: “the mid index is then flattened into two dimensions, x and y, where x is the row number (quotient) and y is the column number (remainder)”.

Using this insight, we can binary-search over the range `[0 .. m*n - 1]`.  At each step, compute `mid = (low + high)/2`, then let `r = mid / n`, `c = mid % n` to access `matrix[r][c]`.  Compare that to the target: if it equals, return true; if it’s less, move `low = mid + 1`; if more, `high = mid - 1`.  This treats the matrix as a linear sorted list.  As one write-up summarizes, this “effectively flattens the 2D search space into 1D, allowing us to utilize the time efficiency of binary search in a multidimensional context”.  The time complexity remains O(log(m\*n)).

Here is a Java implementation of this flattened approach:

```java
public class Search2DMatrix {
    // Search target in a row-major sorted 2D matrix
    public static boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        int m = matrix.length, n = matrix[0].length;
        int low = 0, high = m * n - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            int r = mid / n;
            int c = mid % n;
            int midVal = matrix[r][c];
            if (midVal == target) {
                return true;
            } else if (midVal < target) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1,  3,  5},
            {7,  9, 11},
            {13,15, 17}
        };
        System.out.println(searchMatrix(matrix, 9));  // true
        System.out.println(searchMatrix(matrix, 6));  // false
    }
}
```

In this code, each step “converts the middle index into two dimensions” by dividing and taking a remainder.  If the target exists, the algorithm will find it in at most `O(log(m*n))` time.  This pattern is especially common on coding tests (e.g. LeetCode 74) and is a powerful generalization: any sorted matrix where you can index it in row-major order can be searched this way.

## Pitfalls and Edge Cases

Even experienced programmers can make subtle mistakes in binary search.  Here are common pitfalls and edge cases to watch for:

* **Integer overflow:** Computing the midpoint as `(low + high) / 2` can overflow if `low+high` exceeds the integer limit.  Always use `mid = low + (high - low) / 2` to be safe, especially in languages like Java with fixed-size integers.
* **Infinite loops (off-by-one):** A loop condition like `while (low < high)` may fail to handle the last element correctly and can loop infinitely for small intervals.  For example, if `low=0` and `high=1`, computing `mid=0` repeatedly without adjusting properly will never terminate.  Ensuring the loop uses `<=` and carefully updating bounds (as in our examples) avoids this.  The USACO guide illustrates that using `(low+high)/2` with a strict `<` loop can cause an infinite loop, suggesting alternatives like `(low+high+1)/2` in some cases.
* **Empty input:** Always check if the array (or matrix) is empty before searching.  If `n=0`, immediately return not found. In the 2D case, also check if each row has columns.
* **Target not present:** Design the method to return a sentinel (e.g. `-1` or `false`) when the loop finishes without finding the target.  Do not assume the target will always be found.  In Java’s `Arrays.binarySearch`, the return is negative if not found. In our examples above, we explicitly return -1 or false if the search fails.
* **Duplicates:** As noted, when duplicates exist, basic binary search may return any one copy.  Use the first/last-occurrence variants described above to find boundaries.
* **Bounds errors:** When adapting binary search to other problems (like lower\_bound/upper\_bound), carefully derive the conditions.  For instance, to find the first element ≥ target, treat `arr[mid] >= target` as “move left”, similar to first occurrence logic.
* **Recursive stack depth:** In rare cases with extremely large arrays (size > 2^30), recursive depth (`O(log n)`) might exceed the default stack limits. Iterative search avoids this issue.

By being mindful of these, one can avoid common mistakes.  For interview practice, it’s good to test binary search implementations on small edge cases (arrays of size 0,1,2; element at ends, element not in array, all elements equal, etc.) to ensure correctness.

# References

Our explanations and code patterns above are based on well-known algorithm descriptions and best practices. These include GeeksforGeeks tutorials, algorithm guides, and documented examples. Each concept (from simple binary search to matrix flattening) is supported by these sources.
Citations

geeksforgeeks.org
Binary Search Algorithm - Iterative and Recursive Implementation - GeeksforGeeks
Binary Search Algorithm is a searching algorithm used in a sorted array by r epeatedly dividing the search interval in half. The idea of binary search is to use the information that the array is sorted and reduce the time complexity to O(log N).

geeksforgeeks.org
Time and Space Complexity Analysis of Binary Search Algorithm - GeeksforGeeks
Time complexity of Binary Search is O(log n), where n is the number of elements in the array. It divides the array in half at each step. Space complexity is O(1) as it uses a constant amount of extra space.

geeksforgeeks.org
Binary Search Algorithm - Iterative and Recursive Implementation - GeeksforGeeks
int low = 0, high = arr.length - 1; while (low <= high) { int mid = low + (high - low) / 2;

usaco.guide
Binary Search · USACO Guide
This results in an infinite loop if `left=0` and `right=1`! To fix this, set `middle = (left+right+1)/2` instead.

geeksforgeeks.org
Find first and last positions of an element in a sorted array - GeeksforGeeks
> The idea is to find the first and last occurrence of a given number separately using binary search.

youcademy.org
Real-World Applications of Binary Search Algorithm
* Index Searching: Binary search can help quickly locate relevant rows in database indexes, especially in B-trees and B+ trees, avoiding full table scans. This is particularly useful in large databases where scanning the entire dataset would be prohibitively slow. By leveraging the sorted nature of indexes, binary search reduces the time complexity from `O(n)` to `O(log n)`, making it

youcademy.org
Real-World Applications of Binary Search Algorithm
* Version Control (Git Bisect): It systematically narrows down the commit that introduced a bug by eliminating half the possibilities with each test. This method, known as “bisecting,” is a powerful tool in software development, enabling developers to quickly identify the source of a regression by testing a minimal set of commits.

algo.monster
74. Search a 2D Matrix - In-Depth Explanation
* Index Flattening: The mid index is then flattened into two dimensions, `x` and `y`, where `x` is the row number and `y` is the column number. This is achieved using the `divmod` function, where `mid` is divided by the number of columns `n`, `x` takes the quotient, and `y` takes the remainder of this division.

algo.monster
74. Search a 2D Matrix - In-Depth Explanation
This binary search algorithm effectively flattens the 2D search space into 1D, allowing us to utilize the time efficiency of binary
