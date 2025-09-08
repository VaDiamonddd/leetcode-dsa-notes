# 03. Sliding Window

**Why it matters.** Lets you process **contiguous ranges** (subarrays/substrings) in **O(n)** time by moving two indices (`left`, `right`) and maintaining a small amount of state (sum, counts, product, etc.).

**What is a subarray?** A contiguous slice of an array (or substring of a string). A subarray/window is fully defined by its **bounds**: `[left, right]`.

---

## When to use (decision checklist)

- The task talks about **subarrays/substrings** and a notion of **valid vs invalid** based on:
  - a **constraint metric** (sum, product, count of zeros/unique chars, freq of a char, etc.)
  - a **numeric restriction** (≤ k, < k, = k, at most k)
- You need one of:
  - **Best window** (max/min length/value among valid windows)
  - **Count of valid windows**
  - **Fixed window size k** (compute a metric over every length-k window)

If data are **all positive** (numbers) or the metric is **monotone** w.r.t. window growth, classic sliding window works directly.  
If you need character frequencies/distinct counts → pair window with a **hash map**.

---

## Core idea

- Maintain two indices: `left` (start), `right` (end, inclusive).
- Expand by moving `right` forward and **add** its contribution to a running state `curr`.
- While window is **invalid**, move `left` forward and **remove** its contribution.
- After fixes, window is valid → **update the answer**.
- Each element enters/leaves at most once ⇒ total moves ≤ `2n` ⇒ **O(n)** time, **O(1)** aux space (plus any small map).

---

## Skeletons

### A) General dynamic window (find best valid window)
```pseudo
left = 0
curr = <state>   // sum / product / freq map / distinct count / etc.
best = <identity>

for right in 0..n-1:
    add(arr[right], curr)

    while not isValid(curr):      // shrink until valid
        remove(arr[left], curr)
        left += 1

    best = combine(best, windowValue(left, right, curr))  // e.g., maxLen = max(maxLen, right-left+1)

return best
