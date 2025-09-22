# Prefix Sum

## What is a Prefix Sum?

For an array `nums`, a **prefix sum** array `prefix` stores cumulative sums:

* Inclusive style: `prefix[i] = nums[0] + ... + nums[i]`
* Example: `nums = [5,2,1,6,3,8]` → `prefix = [5,7,8,14,17,25]`

This lets you get any subarray sum in **O(1)** after **O(n)** pre-processing.

### Subarray sum formulas

If `prefix[i]` is inclusive:

```
sum(i..j) = prefix[j] - prefix[i] + nums[i]
```

Safer and often cleaner is the length-(n+1) variant:

```
prefix[0] = 0
prefix[i+1] = prefix[i] + nums[i]        // i from 0..n-1
sum(i..j) = prefix[j+1] - prefix[i]      // handles i = 0 without special cases
```

---

## Why it works (in one line)

`prefix[j]` is sum up to `j`; subtract the sum up to just before `i`, and you’re left with exactly the elements `i..j`.

---

## Building Prefix Sum

**Inclusive style (size n):**

```java
int n = nums.length;
int[] prefix = new int[n];
prefix[0] = nums[0];
for (int i = 1; i < n; i++) {
    prefix[i] = prefix[i - 1] + nums[i];
}
```

**Length-(n+1) style (recommended):**

```java
int n = nums.length;
long[] prefix = new long[n + 1];    // long helps avoid overflow
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}
// sum(i..j) = prefix[j + 1] - prefix[i]
```

---

## Example 1: Range Sum Queries vs a Limit

**Problem.** Given `nums`, queries `[[x,y], ...]`, and `limit`, return a boolean per query: `true` if `sum(nums[x..y]) < limit`.

**Prefix-sum solution (inclusive style, as in the article):**

```java
public boolean[] answerQueries(int[] nums, int[][] queries, int limit) {
    int n = nums.length;
    int[] prefix = new int[n];
    prefix[0] = nums[0];
    for (int i = 1; i < n; i++) prefix[i] = prefix[i - 1] + nums[i];

    boolean[] ans = new boolean[queries.length];
    for (int i = 0; i < queries.length; i++) {
        int x = queries[i][0], y = queries[i][1];
        int sum = prefix[y] - prefix[x] + nums[x];   // inclusive style
        ans[i] = sum < limit;
    }
    return ans;
}
```

**Complexity.** Build `prefix` in `O(n)`. Each query in `O(1)`. Total `O(n + m)` time, `O(n)` space.

> Tip: if values can be large, use `long` for the prefix.

---

## Example 2: 2270. Number of Ways to Split Array

**Task.** Count indices `i` where `sum(0..i) >= sum(i+1..n-1)` and the right side is non-empty.

### Version A: With prefix array

```java
class Solution {
    public int waysToSplitArray(int[] nums) {
        int n = nums.length;
        long[] prefix = new long[n];
        prefix[0] = nums[0];
        for (int i = 1; i < n; i++) prefix[i] = prefix[i - 1] + nums[i];

        int ans = 0;
        for (int i = 0; i < n - 1; i++) {            // right side must be non-empty
            long left = prefix[i];
            long right = prefix[n - 1] - prefix[i];
            if (left >= right) ans++;
        }
        return ans;
    }
}
```

### Version B: O(1) extra space

Compute `total` once, then sweep:

```java
class Solution {
    public int waysToSplitArray(int[] nums) {
        long total = 0, left = 0;
        for (int x : nums) total += x;

        int ans = 0;
        for (int i = 0; i < nums.length - 1; i++) {  // ensure right side non-empty
            left += nums[i];
            long right = total - left;
            if (left >= right) ans++;
        }
        return ans;
    }
}
```

**Complexity.** `O(n)` time, `O(1)` extra space.

---

## When to use prefix sums

* Many subarray sum queries on a fixed array.
* Need to compare sums of two sections repeatedly.
* Pre-processing once to make later queries constant time.

---

## Common pitfalls

* Off-by-one: prefer the `(n+1)` prefix form to avoid special-casing `i = 0`.
* Overflow: build `prefix` as `long[]` if sums can exceed `int`.
* Be clear about **inclusive** vs **exclusive** definitions and keep formulas consistent.

---

## Copy-ready snippet: `(n+1)` prefix utility

```java
final class PrefixSum {
    private final long[] pref;  // pref[0] = 0, pref[i+1] = sum of nums[0..i]

    PrefixSum(int[] nums) {
        pref = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            pref[i + 1] = pref[i] + nums[i];
        }
    }

    // Sum of nums[l..r], inclusive. Requires 0 <= l <= r < n.
    long rangeSum(int l, int r) {
        return pref[r + 1] - pref[l];
    }

    // Total sum
    long total() {
        return pref[pref.length - 1];
    }
}
```

---

## TL;DR

* Build once in `O(n)`, answer subarray sums in `O(1)`.
* Use `(n+1)`-length prefix with `long`.
* For “ways to split,” you can skip the array: keep `total` and a running `left`.
