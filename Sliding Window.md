

# Sliding Window Technique 

The **sliding window** is a common algorithmic method for efficiently processing contiguous subarrays or substrings.  It transforms the brute-force double-loop approach into a single loop by “sliding” a window (of fixed or variable size) over the data.  In practice, we maintain some running state (like a sum or counts) for the current window, then update it incrementally as the window moves.  This reduces time complexity (often from $O(nk)$ or $O(n^2)$ down to $O(n)$). For example, to find the maximum sum of any subarray of length $k$, we compute the sum of the first $k$ elements, then slide the window one step at a time by subtracting the element that leaves and adding the new element that enters.  As Gul Ershad explains, “The sliding window algorithm is a problem-solving technique that transforms two nested loops into one loop,” lowering the complexity to $O(n)$ for many problems.  Problems suited to this technique often involve finding some optimum (maximum, minimum, longest, shortest, or count) over every contiguous subarray or substring of a sequence.  For example, the figure below shows a window of size 3 sliding over the array `[5, 2, -1, 0, 3]`:

&#x20;*Figure: Sliding a fixed-size window (size = 3) over an array. Initially the window covers `[5,2,-1]` (sum = 6). Then it slides right by one to `[2,-1,0]`, etc. (Illustration adapted from.)*

In the figure above, the first window covers `5,2,-1` (sum = 6).  When we slide the window one step to the right, it drops `5` and includes `0`, yielding the new window `[2,-1,0]` (sum = 1).  We can update the running sum by doing `sum += newElement - oldElement`.  This single-loop approach (instead of a nested loop) is the core of sliding-window algorithms. Below we explore several examples, from basic to advanced, all implemented in Java. Each example includes a problem statement, an explanation of the sliding-window approach, Java code, and a time/space analysis.

## Maximum Sum Subarray of Size K

**Problem:** Given an integer array `arr[]` and an integer `k`, find the maximum sum of any contiguous subarray of length exactly `k`.  For example, if `arr = [100, 200, 300, 400]` and `k = 2`, the answer is `700` (the subarray `[300,400]`).  If the array length is less than `k`, the problem is invalid.

**Sliding-Window Approach:** First compute the sum of the initial window of size `k` (elements `arr[0]` through `arr[k-1]`). Call this `window_sum`. Set `max_sum = window_sum`. Then slide the window through the array: at each step `i = k..n-1`, update

```
window_sum = window_sum + arr[i] - arr[i-k];
```

This effectively removes the element leaving the window (`arr[i-k]`) and adds the new entering element (`arr[i]`).  After each update, set `max_sum = max(max_sum, window_sum)`.  This way each window is handled in **O(1)** time from the previous one, yielding an overall **O(n)** algorithm.

```java
// Returns maximum sum of any subarray of size k
static int maxSumSubarray(int[] arr, int k) {
    int n = arr.length;
    if (n < k) return -1; // not enough elements
    // Compute sum of first window of size k
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    int maxSum = windowSum;
    // Slide the window from i=k to n-1
    for (int i = k; i < n; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}

// Example usage
public static void main(String[] args) {
    int[] arr = {5, 2, -1, 0, 3};
    int k = 3;
    System.out.println(maxSumSubarray(arr, k));  // Output: 6
}
```

**Explanation:** We first sum `[5, 2, -1] = 6` (initial window).  Then for each step we do `window_sum += new - old`.  For example, slide to include `0` and drop `5`: new sum `= 6 + 0 - 5 = 1`.  Then slide again (drop `2`, add `3`): `window_sum = 1 + 3 - 2 = 2`. The maximum seen is `6`.  This method uses only constant extra space and runs in **O(n)** time (each element is added and removed exactly once).

* **Time Complexity:** $O(n)$ – each element is processed once in the window updates.
* **Space Complexity:** $O(1)$ – only a few variables for sums are used.

## First Negative Integer in Every Window of Size K

**Problem:** Given an integer array `arr[]` and a window size `k`, for each contiguous subarray (window) of length `k` report the **first negative** integer in that window (or 0 if none exists).  Example: `arr = [-8, 2, 3, -6, 10]`, `k = 2`.  The windows are `[-8,2]`, `[2,3]`, `[3,-6]`, `[-6,10]`; the first negatives are `-8, 0, -6, -6` respectively.

**Sliding-Window Approach (Deque):** A deque can efficiently track which negatives are in the current window. We process the array from left to right, maintaining a deque of indices of negative elements in the **current** window.  At step `i` (for the new element `arr[i]`):

* If `arr[i]` is negative, push index `i` onto the back of the deque.
* Remove any indices from the front of the deque that are now out of the window (less than `i-k+1`).
* Now the front of the deque, if any, is the index of the first negative in the current window.  If the deque is empty, the window has no negative (report 0).

This ensures each element is added/removed once, for **O(n)** time overall.  The deque always stores indices of “useful” elements (negatives) sorted by index; the front holds the earliest negative in the window.

```java
static int[] firstNegativeInWindow(int[] arr, int k) {
    int n = arr.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> dq = new ArrayDeque<>();
    
    // Initialize deque with first window
    for (int i = 0; i < k; i++) {
        if (arr[i] < 0) {
            dq.addLast(i);
        }
    }
    // Process all windows
    for (int i = k; i < n; i++) {
        // Record result for the previous window
        result[i - k] = dq.isEmpty() ? 0 : arr[dq.peekFirst()];
        
        // Remove indices out of the new window
        while (!dq.isEmpty() && dq.peekFirst() <= i - k) {
            dq.pollFirst();
        }
        // Add current element if negative
        if (arr[i] < 0) {
            dq.addLast(i);
        }
    }
    // Last window result
    result[n - k] = dq.isEmpty() ? 0 : arr[dq.peekFirst()];
    return result;
}

// Example usage
public static void main(String[] args) {
    int[] arr = {12, -1, -7, 8, -15, 30, 16, 28};
    int k = 3;
    System.out.println(Arrays.toString(firstNegativeInWindow(arr, k)));
    // Output: [-1, -1, -7, -15, -15, 0]
}
```

**Explanation:** We build the deque for the first window and then slide.  The code from GeeksforGeeks explains: “We create a Deque dq of capacity k, storing only useful elements (negatives) of the current window. For a given window, if dq is not empty then the element at the front of dq is the first negative integer for that window”.  For example, with `arr=[12,-1,-7,8,-15,30,16,28]` and `k=3`:

* Initial window `[12,-1,-7]`, deque stores indices of `-1` and `-7` (front= index1 with value -1), so result = `-1`.

* Slide to `[-1,-7,8]`: first window element `12` left, but deque front was index 1 (still in window), so result = `-1`.

* Slide to `[-7,8,-15]`: remove index1 (out-of-window), deque front is now index2 (value -7), result = `-7`.
  …and so on. Each window result is obtained in constant time from the deque.

* **Time Complexity:** $O(n)$ – each element index is added and removed at most once from the deque.

* **Space Complexity:** $O(k)$ – deque stores at most $k$ indices in the worst case.

## Longest Substring Without Repeating Characters

**Problem:** Given a string `s`, find the length of the longest substring in which all characters are unique (no repeats).  Example: if `s = "geeksforgeeks"`, the longest substrings without repeating letters include `"eksforg"` (length 7).

**Sliding-Window Approach:** This is a classic variable-size window problem. We use two pointers `left` and `right` (initially 0) and a boolean array or map to mark which characters are in the current window. We expand the right pointer one step at a time, adding new characters. If the new character `s[right]` is **not** already in the window (unvisited), we mark it visited and update the best length. If it *is* already present (a repeat), we shrink the window from the left until the repeated character is removed. This keeps the substring between `left` and `right` always free of duplicates. As GeeksforGeeks describes: “If the character at right pointer is visited, \[…] the left pointer moves to the right while marking visited characters as false until the repeating character is no longer part of the window”. Throughout, we track `max_len = max(max_len, right-left+1)`.

```java
static int lengthOfLongestUniqueSubstring(String s) {
    int n = s.length();
    boolean[] seen = new boolean[256];  // or use 128 for ASCII
    int maxLen = 0;
    int left = 0, right = 0;
    while (right < n) {
        char c = s.charAt(right);
        // If repeating char, shrink from left
        while (seen[c]) {
            seen[s.charAt(left)] = false;
            left++;
        }
        // Include s[right] in window
        seen[c] = true;
        // Update max length
        maxLen = Math.max(maxLen, right - left + 1);
        right++;
    }
    return maxLen;
}

// Example usage
public static void main(String[] args) {
    String s = "geeksforgeeks";
    System.out.println(lengthOfLongestUniqueSubstring(s));  // Output: 7
}
```

**Explanation:** We maintain a window `[left, right)` of unique characters. When `s[right]` is already in the window, we move `left` up until it’s removed (marking seen false). For example, in `"geeksforgeeks"`, as soon as the second `'e'` appears, we shrink from the left to drop the first `'e'`.  This “two-pointer” method ensures each character is visited at most twice, yielding **O(n)** time.

* **Time Complexity:** $O(n)$ – each character is added and removed at most once from the window.
* **Space Complexity:** $O(1)$ – the auxiliary array/map size is fixed (e.g. 256 booleans).

## Minimum Window Substring

**Problem:** Given two strings `s` and `t`, find the smallest (minimum-length) substring in `s` that contains all the characters of `t` (including duplicates).  If no such window exists, return the empty string.  Example: `s = "ADOBECODEBANC"`, `t = "ABC"`, the answer is `"BANC"` since it includes A, B, and C and is the shortest such substring.

**Sliding-Window Approach:** This is a more advanced variable window problem. We use two hash maps (or count arrays) to track character frequencies: one for the required counts from `t` (`need`), and one for the current window in `s` (`window`). We expand the right end of the window until all characters from `t` are included (i.e. the window’s counts meet or exceed `need` for every character). Then we try to shrink the left end to drop any extra characters, updating the minimum whenever the window still covers all of `t`. As AlgoMonster explains, “the main idea revolves around expanding and shrinking a window on `s` to include the characters from `t` in the most efficient way possible”. In code, we increment a counter `formed` whenever adding a character from `t` helps satisfy the count, and whenever `formed == t.length()`, we know the window contains all of `t`. Then we move the left pointer up while maintaining this condition, updating our best window if smaller.

```java
public String minWindow(String s, String t) {
    if (s.isEmpty() || t.isEmpty()) return "";
    // Frequency arrays for characters (assuming ASCII)
    int[] need = new int[128];
    int[] window = new int[128];
    for (char c : t.toCharArray()) {
        need[c]++;
    }
    int have = 0;             // how many target chars currently matched
    int left = 0, minLen = Integer.MAX_VALUE, minStart = 0;
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        window[c]++;
        // If c is needed and count is not exceeded, increment have
        if (window[c] <= need[c]) {
            have++;
        }
        // When all chars from t are covered, try to shrink
        while (have == t.length()) {
            // Update best window
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                minStart = left;
            }
            // Remove s[left] from window and move left forward
            char leftChar = s.charAt(left);
            window[leftChar]--;
            if (window[leftChar] < need[leftChar]) {
                have--;
            }
            left++;
        }
    }
    return (minLen == Integer.MAX_VALUE) ? "" : s.substring(minStart, minStart + minLen);
}

// Example usage
public static void main(String[] args) {
    String s = "ADOBECODEBANC", t = "ABC";
    System.out.println(new Solution().minWindow(s, t));  // Output: "BANC"
}
```

**Explanation:** We keep a sliding window `[left, right]` and two counters.  As we expand `right`, we track when we've included enough of each character in `t`.  Once all needed characters are present (i.e. `have == t.length()`), we move `left` forward to drop unnecessary characters, updating the minimum-length answer when the window is valid. This method is guaranteed to examine each character of `s` a constant number of times (expand or contract), giving **O(n + m)** time where $n = |s|$ and $m = |t|$.

* **Time Complexity:** $O(n + m)$ – each character of `s` is visited at most twice (once by right, once by left).
* **Space Complexity:** $O(1)$ – we use fixed-size arrays/maps for character counts (or $O(n)$ if considering the output string size).

## Count Distinct Elements in Each Window

**Problem:** Given an integer array `arr[]` and window size `k`, for each contiguous subarray of length `k`, count the number of distinct elements.  Return the list of these counts for all windows.  Example: `arr = [1, 2, 1, 3, 4, 2, 3]`, `k = 4`. The windows are `[1,2,1,3]`, `[2,1,3,4]`, `[1,3,4,2]`, `[3,4,2,3]`, with distinct counts `3,4,4,3` respectively.

**Sliding-Window Approach:** We maintain a frequency map (`HashMap<Integer,Integer>`) for the current window. First, build the map for the first `k` elements and record its size (the count of keys) as the first result. Then slide the window: for the new element entering the window, increment its count in the map; for the element leaving the window, decrement its count. If a count drops to zero, remove that key. The number of distinct elements in the window is just the map’s size. Repeat this for all windows. This works in linear time with $O(k)$ space for the map.

```java
static List<Integer> countDistinctEachWindow(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    Map<Integer,Integer> freq = new HashMap<>();
    // First window
    for (int i = 0; i < k; i++) {
        freq.put(arr[i], freq.getOrDefault(arr[i], 0) + 1);
    }
    res.add(freq.size());
    // Slide the window
    for (int i = k; i < n; i++) {
        // Add new element
        freq.put(arr[i], freq.getOrDefault(arr[i], 0) + 1);
        // Remove old element
        int old = arr[i - k];
        freq.put(old, freq.get(old) - 1);
        if (freq.get(old) == 0) {
            freq.remove(old);
        }
        // Current distinct count
        res.add(freq.size());
    }
    return res;
}

// Example usage
public static void main(String[] args) {
    int[] arr = {1, 2, 1, 3, 4, 2, 3};
    int k = 4;
    System.out.println(countDistinctEachWindow(arr, k));
    // Output: [3, 4, 4, 3]
}
```

**Explanation:** We keep a sliding frequency map of size at most $k$.  When we move the window by one, we add the new element’s count and decrement the outgoing element’s count, removing it entirely if it hits zero.  The map’s size gives the number of distinct elements.  For the example above, after the first window `[1,2,1,3]` the map has `{1:2,2:1,3:1}`, size 3.  Sliding to `[2,1,3,4]`, we add `4` and remove one `1`, resulting map `{1:1,2:1,3:1,4:1}`, size 4, and so on.

* **Time Complexity:** $O(n)$ – each element is added/removed once.
* **Space Complexity:** $O(k)$ – the map holds at most $k$ entries.

## Maximum of All Subarrays of Size K

**Problem:** Given an integer array `arr[]` and an integer `k`, find the **maximum** element in every contiguous subarray of size `k`.  Return these maxima in order.  Example: `arr = [1, 3, 2, 1, 7, 3]`, `k = 3`.  The windows are `[1,3,2]`, `[3,2,1]`, `[2,1,7]`, `[1,7,3]` with maximums `[3,3,7,7]`.

**Sliding-Window Approach (Deque):** We use a deque to store indices of “useful” elements in decreasing order of values. Process elements from left to right:

* For each new index `i`, first remove indices from the **back** of the deque while `arr[i] >= arr[dq.peekLast()]` (those smaller are useless since `arr[i]` would dominate).
* Then add index `i` to the back.
* Remove indices from the **front** if they are out of the current window (`<= i-k`).
* The front of the deque always holds the index of the current window’s maximum.

GeeksforGeeks describes this: “Create a Deque… storing only useful elements of current window… maintained in sorted order. The element at front of dq is the largest of the current window”.  This algorithm also runs in **O(n)** time since each element is added/removed at most once.

```java
static List<Integer> maxOfAllSubarrays(int[] arr, int k) {
    int n = arr.length;
    List<Integer> res = new ArrayList<>();
    Deque<Integer> dq = new ArrayDeque<>();
    // Process first window
    for (int i = 0; i < k; i++) {
        while (!dq.isEmpty() && arr[i] >= arr[dq.peekLast()]) {
            dq.pollLast();
        }
        dq.addLast(i);
    }
    // Process rest of the windows
    for (int i = k; i < n; i++) {
        // Front of deque is max of previous window
        res.add(arr[dq.peekFirst()]);
        // Remove elements out of this window
        while (!dq.isEmpty() && dq.peekFirst() <= i - k) {
            dq.pollFirst();
        }
        // Add current element, removing smaller ones
        while (!dq.isEmpty() && arr[i] >= arr[dq.peekLast()]) {
            dq.pollLast();
        }
        dq.addLast(i);
    }
    // Max of last window
    res.add(arr[dq.peekFirst()]);
    return res;
}

// Example usage
public static void main(String[] args) {
    int[] arr = {1, 3, 2, 1, 7, 3};
    int k = 3;
    System.out.println(maxOfAllSubarrays(arr, k));
    // Output: [3, 3, 7, 7]
}
```

**Explanation:** The deque stores indices whose values are in descending order.  For window `[1,3,2]`, the deque will have `[1,3]` (actually storing indices of 3 and 1), so max = 3.  Slide to `[3,2,1]`: remove out-of-window index (if any) and update, max stays 3.  For `[2,1,7]`, when adding `7`, we pop all smaller values (2 and 1) and the deque becomes just `[7]`, so max = 7.  Detailed steps are given in GeeksforGeeks.

* **Time Complexity:** $O(n)$ – each element is added and removed at most once from the deque.
* **Space Complexity:** $O(k)$ – deque stores at most $k$ indices.

