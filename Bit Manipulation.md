# ⚙️ Advanced Bit Manipulation Techniques for Competitive Programming

Bit manipulation involves treating integers as arrays of bits and using bitwise operations for efficient computation. In competitive programming, mastery of bit tricks can lead to elegant, low-level optimizations. Below we cover several advanced bit-manipulation concepts with detailed explanations, example use-cases, and representative Java problems (with solutions) to illustrate each idea.

---

## 📌 Subset Generation Using Bitmask

Every subset of an n-element set can be represented by an n-bit binary number (a bitmask) from 0 to 2ⁿ−1, where bit j indicates inclusion of the j-th element. To generate the power set, loop mask from 0 to (1<\<n)-1, and for each bit j check `(mask & (1<<j)) != 0` to include element j. This iterates all subsets in O(2ⁿ·n) time. For example, mask 5 (binary 101) selects elements 0 and 2. This method naturally handles sorted ordering of bits. When duplicates exist in the input, one can generate all bitmasks and then use a set to filter unique subsets.

**Example Problem (Subsets):**
Given an array of distinct integers, return all possible subsets.
**Solution:** We use a loop from 0 to (1<\<n)-1 and build each subset by checking bits. This is straightforward and runs in O(2ⁿ·n).

```java
public List<List<Integer>> subsets(int[] nums) {
    int n = nums.length;
    List<List<Integer>> result = new ArrayList<>();
    // Iterate over all bitmasks from 0 to 2^n - 1
    for (int mask = 0; mask < (1 << n); mask++) {
        List<Integer> subset = new ArrayList<>();
        for (int j = 0; j < n; j++) {
            // If j-th bit of mask is set, include nums[j]
            if ((mask & (1 << j)) != 0) {
                subset.add(nums[j]);
            }
        }
        result.add(subset);
    }
    return result;
}
```

**Logic & Performance:**
Each integer mask encodes a subset. Checking bits is constant-time per bit, so overall time is O(2ⁿ·n), which is optimal for generating all subsets. This code assumes distinct input; handling duplicates would require sorting and skipping repeated combinations or using a `Set<List<Integer>>` to eliminate duplicates.

---

## ❌ XOR Tricks and Unique Elements

The XOR operator (`^`) has useful properties: `X^X = 0` and `X^0 = X`, and it is commutative. Therefore, XOR-ing an array where all numbers appear twice except one leaves the unique number (all pairs cancel out). More generally, XOR can identify differing bits or swap values.

* **Finding a single unique element:** XOR all elements; the result is the element that appears once.
* **Finding two unique elements:** First XOR all to get `xor = a^b`. Then isolate a set bit of xor (e.g. `rightMost = xor & -xor`) to partition numbers into two groups and XOR each.

**Example Problem (Single Number):**
Every element appears twice except one. Find the unique element.
**Solution:** Iterate and XOR. This runs in O(n) time and O(1) space

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;  // pairwise cancellation
    }
    return result;
}
```

**Logic & Performance:**
By XOR-ing all elements, pairs cancel (`X^X=0`), leaving the single element. This is O(n) time, very efficient for large arrays.

---

## 🧩 Bit Masking and Toggling

Bit masks allow selective manipulation of bits:

* **Set bit k to 1:** `n | (1 << k)` (OR with a mask having only bit k set)
* **Clear bit k to 0:** `n & ~(1 << k)` (AND with the complement of mask)
* **Toggle (flip) bit k:** `n ^ (1 << k)` (XOR with mask flips that bit)

Toggling is especially useful: for example, swapping two bits in an integer can be done using XOR. If the bits at positions i and j differ, XORing with `((1<<i)|(1<<j))` will swap them.

**Example Problem (Swap Bits):**
Given an integer `x` and positions `i` and `j`, swap the bits at those positions.
**Solution:** Check if bits differ; if so, flip both. This uses XOR masks and runs in O(1) time.

```java
public int swapBits(int x, int i, int j) {
    // Check if the i-th and j-th bits are different
    if (((x >> i) & 1) != ((x >> j) & 1)) {
        // Create mask with bits i and j set, then XOR to flip them
        int mask = (1 << i) | (1 << j);
        x ^= mask;  // toggle both bits
    }
    return x;
}
```

**Logic & Performance:**
We isolate bits by shifting and mask with `&1` to check. If they differ, toggling both via `x ^= (1<<i)|(1<<j)` swaps them. The operation is constant-time and uses just a few bitwise ops. This is very efficient (O(1)) for any given positions.

---

## 🔢 Counting Set Bits Efficiently

A classic trick to count set bits (Hamming weight) is Brian Kernighan’s algorithm: repeatedly do `n &= (n-1)` which clears the lowest set bit each time. Count iterations until `n` becomes 0. This loops only once per set bit, giving O(k) time where k is the number of 1-bits (often much faster than the O(log n) naive method).

**Example Problem (Hamming Weight):**
Given a 32-bit integer, count its 1-bits.
**Solution:** Use Kernighan’s method.

```java
public int hammingWeight(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1);  // clear the lowest set bit
        count++;
    }
    return count;
}
```

**Logic & Performance:**
Each iteration removes one set bit using `n &= (n-1)`, so the loop executes as many times as there are 1s. For a 32-bit integer, the worst-case is 32 iterations, which is effectively constant. This beats checking each bit individually when n has few bits set. Java also offers `Integer.bitCount(n)` as a built-in optimized method.

---

## 🔍 Checking Power of Two or Four

* **Power of Two:** A positive integer is a power of two if and only if it has exactly one bit set. Equivalently, `(n & (n-1)) == 0` when `n > 0`. This is O(1).
* **Power of Four:** A positive power-of-four number is also a power of two and its single set bit is in an odd position (1-based). We can check: `(n & (n-1)) == 0` and `(n & 0x55555555) != 0`. The mask `0x55555555` (binary 0101…0101) has 1s in all odd positions, so `n & 0x55555555` tests that the set bit is at an odd index.

**Example Problem (Power of Four):**
Determine if an integer is a power of four (greater than 0).
**Solution:** Combine the power-of-two check with the mask check.

```java
public boolean isPowerOfFour(int n) {
    if (n <= 0) return false;
    // Check that n is a power of two (only one bit set)
    if ((n & (n - 1)) != 0) return false;
    // Mask for bits in odd positions (1,3,5,...): 0x55555555 in hex
    return (n & 0x55555555) != 0;
}
```

**Logic & Performance:**
First ensure `n` is positive and has only one set bit (`n&(n-1)==0`). Then the mask check (`n & 0x55555555)!=0`) ensures that the single 1-bit is in an odd position (bits 1,3,5,…). Both operations are constant-time bitwise checks. This runs in O(1) time and O(1) space. (For power of two alone, one would only use the first check.)

---

## 📍 Finding the Rightmost Set Bit

To isolate the rightmost 1-bit in `n`, use the expression `n & (-n)` (or equivalently `n & (~n+1)`). This yields a number with only that lowest 1-bit set. For example, if `n = 0b10110000`, then `n & -n = 0b00010000`. To find its position (1-indexed), you can count trailing zeros plus one.

**Example Problem (Rightmost 1-Bit Position):**
Given n, return the 1-based index of its lowest set bit, or 0 if `n = 0`.
**Solution:** Use `n & -n` and then compute index.

```java
public int rightmostSetBitPosition(int n) {
    if (n == 0) return 0;
    // Isolate lowest set bit
    int lowest = n & -n;
    // Compute position: count trailing zeros + 1
    return Integer.numberOfTrailingZeros(lowest) + 1;
}
```

**Logic & Performance:**
The expression `n & -n` isolates the lowest set bit in one step. We then use `Integer.numberOfTrailingZeros()` which counts how many 0-bits follow it. This gives the position (adding 1 for 1-based). All operations are constant-time (at most 32-bit). Alternative: one could loop shifting `lowest` right until it becomes 1.

---

## 🔗 Bitwise AND of Number Range

For two integers `m ≤ n`, the bitwise AND of all numbers in the interval `[m, n]` tends to zero out any bit that changes within the range. A common solution is to find the common leading prefix of `m` and `n`: repeatedly shift both right until they become equal, counting how many shifts; then shift that common value back left. This clears all differing lower bits.

**Example Problem (Range Bitwise AND):**
Compute the bitwise AND of all numbers between `m` and `n` inclusive.
**Solution:** Shift until `m == n`.

```java
public int rangeBitwiseAnd(int m, int n) {
    int shift = 0;
    // Shift both numbers right until they match
    while (m < n) {
        m >>= 1;
        n >>= 1;
        shift++;
    }
    // Left shift back to restore position
    return m << shift;
}
```

**Logic & Performance:**
If `m` and `n` differ, at least the lowest bits will vary across the range, forcing those bits to 0 in the AND. By shifting right until `m == n`, we find the common high-bit prefix. Then shifting left `shift` places yields the result. The loop runs at most 32 times (the bit-width), so the algorithm is O(log n) time and O(1) space. For example, `rangeBitwiseAnd(5,7)` shifts until both become 1 (binary 1), then shifts back 2 positions, giving `100₂ = 4` (AND of `101, 110, 111`).

---

Each of these techniques relies on fundamental bit properties and can greatly optimize operations in low-level code or interview problems. The key is to understand how bits propagate through operations like AND, OR, XOR, and shifts, and to practice recognizing patterns (such as unique element problems or bit prefixes) that map to these tricks.

---

**Sources:** Competitive programming and algorithm references. Each code example above uses standard Java bitwise idioms and comments to clarify logic.

---

