# 02. Two Pointers

**Why it matters.** A universal pattern for arrays/strings (and paired iterables) that compresses many O(n²) scans into **O(n)** with **O(1)** extra space.

**When to use.**
- Compare elements from both ends (palindromes, pair sums in sorted arrays).
- Sweep **two sorted** inputs simultaneously (merge, intersections).
- Advance two indices through one iterable (subsequence checks, deduplication).
- Any situation where each step can **move at least one pointer** closer to termination.

**Core idea.**
Keep two integer indices (`left/right` or `i/j`). In each iteration, do O(1) work, then move one/both pointers **monotonically** toward completion.  
If each move reduces remaining work by ≥1, total steps ≤ length (or ≤ n+m across two arrays).

**Complexity.**  
Time: **O(n)** (or **O(n+m)** for two inputs).  
Space: **O(1)** (ignoring the output array when building a result, which is the standard convention).

---

## Skeletons

### A) Opposite ends (single array/string)
Use when comparing symmetric positions (e.g., palindrome) or searching a pair in **sorted** data.

```pseudo
function fn(arr):
    left = 0
    right = arr.length - 1
    while left < right:
        // problem-specific logic/checks using arr[left], arr[right]
        // decide which pointer(s) to move:
        //   left++  |  right--  |  both
