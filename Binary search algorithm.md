Recursive Binary Search in Java: Advanced Guide

Binary search is a classic divide-and-conquer algorithm for searching a sorted array by repeatedly halving the search space.  At each step, you compare the target with the middle element and discard one half of the array.  This strategy reduces the search range exponentially, yielding a logarithmic time complexity (O(log N)).  In recursive form, binary search repeatedly calls itself on a subarray (left or right half) until the target is found or the subarray is empty.  Because each recursive call processes half as many elements, the time complexity remains O(log N) (best-case O(1) if the middle element is the target).  The space complexity is O(log N) auxiliary due to the recursion stack (each call adds one stack frame), whereas the extra space apart from the stack is O(1).

Time & Space Complexity

Time Complexity: In the worst and average cases, each step halves the range, giving O(log N) time.  The best case occurs if the middle element is the target, O(1).

Space Complexity: Iterative binary search uses O(1) extra space. Recursive binary search also uses only constant extra space for variables, but the recursion stack grows to O(log N) in the worst case.


Overall, recursive binary search offers the same time complexity as iterative (O(log N)), with a minor extra cost of recursion space (O(log N)).

Standard Recursive Binary Search (Java)

A standard recursive binary search checks the middle element and then recursively searches the left or right subarray.  The Java code below (adapted from GeeksforGeeks) returns the index of the target x if found, or -1 if not:

public class BinarySearch {
    // Returns index of x if present in arr[low..high], else -1
    public static int binarySearch(int[] arr, int low, int high, int x) {
        if (high < low) {
            return -1;  // Base case: empty subarray
        }
        int mid = low + (high - low) / 2;
        if (arr[mid] == x) {
            return mid;  // Target found
        }
        // If target is smaller, it must be in left subarray
        if (x < arr[mid]) {
            return binarySearch(arr, low, mid - 1, x);
        }
        // Otherwise, it must be in right subarray
        return binarySearch(arr, mid + 1, high, x);
    }

    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 10, 40};
        int target = 10;
        int index = binarySearch(arr, 0, arr.length - 1, target);
        System.out.println((index != -1)
            ? "Element found at index " + index
            : "Element not present");
    }
}

This code splits the array by computing mid = (low + high)/2.  If arr[mid] equals x, it returns mid.  If x is less, it recurses on the left half; otherwise on the right half.  If the subarray becomes empty (low > high), it returns –1.  The time complexity is O(log N) and the recursion depth is at most O(log N).

Recursive Binary Search – First Occurrence

When the array can have duplicate values, a plain binary search may return any occurrence of the target. To find the first (leftmost) occurrence of x, we modify the recursion:

If arr[mid] == x, check if this is the first occurrence: either mid == 0 or arr[mid-1] < x. If so, return mid.

Otherwise (if not first), recurse on the left subarray to find an earlier occurrence.

If arr[mid] < x, recurse right; if arr[mid] > x, recurse left.


This logic (based on GFG’s approach) ensures we find the leftmost index of x. For example:

public static int findFirst(int[] arr, int low, int high, int x) {
    if (high < low) {
        return -1;
    }
    int mid = low + (high - low) / 2;
    if (arr[mid] == x) {
        // Check if it's the first occurrence
        if (mid == 0 || arr[mid - 1] < x) {
            return mid;
        }
        // Otherwise, search left subarray
        return findFirst(arr, low, mid - 1, x);
    }
    if (arr[mid] < x) {
        // Target must be in right subarray
        return findFirst(arr, mid + 1, high, x);
    }
    // arr[mid] > x: search left subarray
    return findFirst(arr, low, mid - 1, x);
}

This recursively narrows the range.  If mid holds x but isn’t the first occurrence, it searches [low..mid-1].  Otherwise it directs the search based on comparing x with arr[mid].  The time complexity remains O(log N).  The key checks ((mid == 0 || arr[mid-1] < x) && arr[mid] == x) mirror the GFG “expected approach” steps to identify the first occurrence.

Recursive Binary Search – Last Occurrence

Similarly, to find the last (rightmost) occurrence of x, we adjust the checks:

If arr[mid] == x, check if it’s the last occurrence: either mid == n-1 or arr[mid+1] > x. If so, return mid.

Otherwise (not last), recurse on the right subarray to find a later occurrence.

If arr[mid] < x, recurse right; if arr[mid] > x, recurse left.


This ensures the rightmost index of x is found (GFG’s steps). For example:

public static int findLast(int[] arr, int low, int high, int x) {
    if (high < low) {
        return -1;
    }
    int mid = low + (high - low) / 2;
    if (arr[mid] == x) {
        // Check if it's the last occurrence
        if (mid == arr.length - 1 || arr[mid + 1] > x) {
            return mid;
        }
        // Otherwise, search right subarray
        return findLast(arr, mid + 1, high, x);
    }
    if (arr[mid] > x) {
        // Target must be in left subarray
        return findLast(arr, low, mid - 1, x);
    }
    // arr[mid] < x: search right subarray
    return findLast(arr, mid + 1, high, x);
}

This code checks the end conditions similarly to the first-occurrence code.  The conditions (mid == n-1 || arr[mid+1] > x) match GFG’s approach for identifying the last occurrence.  Again, this runs in O(log N) time, with O(log N) recursion depth.

Problem 1: Search in a Rotated Sorted Array (Medium)

Problem: Given a sorted array that has been rotated at an unknown pivot, search for a target key.  If found, return its index; otherwise return -1. The array has distinct values and was originally sorted in ascending order (then rotated). For example, in arr = [5,6,7,8,9,10,1,2,3], the target 3 is at index 8.

To solve this, we use a modified binary search that detects which half is sorted.  At each step, compare arr[mid] with arr[low] to see if the left half [low..mid] is sorted. If the left half is sorted and target lies between arr[low] and arr[mid], recurse into the left half; otherwise recurse into the right half. If the left half isn’t sorted, then the right half [mid..high] must be sorted, and we apply analogous logic. The key checks (“If left half is sorted…” logic) are illustrated in this GFG example:

public static int searchRotated(int[] arr, int low, int high, int key) {
    if (low > high) {
        return -1;
    }
    int mid = low + (high - low) / 2;
    if (arr[mid] == key) {
        return mid;  // Target found
    }
    // If left half [low..mid] is sorted
    if (arr[low] <= arr[mid]) {
        // Check if key lies in left half
        if (key >= arr[low] && key < arr[mid]) {
            return searchRotated(arr, low, mid - 1, key);
        } else {
            return searchRotated(arr, mid + 1, high, key);
        }
    }
    // Otherwise, right half [mid..high] must be sorted
    if (key > arr[mid] && key <= arr[high]) {
        return searchRotated(arr, mid + 1, high, key);
    } else {
        return searchRotated(arr, low, mid - 1, key);
    }
}

public static void main(String[] args) {
    int[] arr = {5, 6, 7, 8, 9, 10, 1, 2, 3};
    int target = 3;
    int index = searchRotated(arr, 0, arr.length - 1, target);
    System.out.println((index != -1)
        ? "Found at index " + index
        : "Not found");
}

In this code, after checking arr[mid], we use the condition arr[low] <= arr[mid] to detect a sorted left half.  If sorted, we see if key lies in that range (arr[low] to arr[mid]); if so, we recurse left, otherwise right.  This recursive search takes O(log N) time in the worst case.  (This is the standard solution to “Search in Rotated Sorted Array”, a common medium-level interview problem.)

Problem 2: Find Rotation Count in a Rotated Sorted Array (Medium)

Problem: Given a sorted array that has been rotated, find the number of times it was rotated. Equivalently, find the index of the minimum element. For example, [15, 18, 2, 3, 6, 12] is a sorted array rotated twice, so the answer is 2.

A binary-search solution finds the pivot (minimum) index in O(log N) time by comparing mid with its neighbors. We use the fact that if arr[mid] > arr[mid+1], then mid+1 is the pivot; if arr[mid] < arr[mid-1], then mid is the pivot. Otherwise, decide whether to search left or right by comparing arr[mid] with arr[high]. The recursive Java solution below is taken from GFG:

public class RotationCount {
    // Returns the index of the minimum element
    static int countRotations(int[] arr, int low, int high) {
        // If subarray is not rotated at all
        if (high < low) {
            return 0;
        }
        // If there is only one element left
        if (high == low) {
            return low;
        }
        int mid = low + (high - low) / 2;
        // Check if element (mid+1) is the minimum
        if (mid < high && arr[mid + 1] < arr[mid]) {
            return mid + 1;
        }
        // Check if mid itself is the minimum
        if (mid > low && arr[mid] < arr[mid - 1]) {
            return mid;
        }
        // Decide which side to search
        if (arr[high] > arr[mid]) {
            // Minimum is in left subarray
            return countRotations(arr, low, mid - 1);
        }
        // Otherwise, minimum is in right subarray
        return countRotations(arr, mid + 1, high);
    }

    public static void main(String[] args) {
        int[] arr = {15, 18, 2, 3, 6, 12};
        System.out.println("Rotation count = " 
            + countRotations(arr, 0, arr.length - 1));
    }
}

Here, countRotations recursively narrows down the subarray.  It handles two base cases (high < low and one-element left).  It then checks if arr[mid] or arr[mid+1] is the pivot (the minimum element). If neither, it compares arr[mid] with arr[high] to decide direction.  The result is the index of the smallest element, which equals the rotation count. This recursive solution also runs in O(log N) time.

Summary: We’ve covered recursive binary search and its variations.  All recursive implementations run in logarithmic time, with auxiliary recursion stack O(log N).  The standard binary search finds any occurrence of a target; small modifications let us find the first or last occurrence of a duplicate target in a sorted array.  Advanced interview problems often use binary search logic on modified inputs (e.g. rotated arrays).  The above examples illustrate how to apply recursion to such problems efficiently.

Sources: Authoritative algorithm references and coding examples from GeeksforGeeks and related sources were used to explain these techniques.

