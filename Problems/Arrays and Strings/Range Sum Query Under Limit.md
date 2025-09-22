# Range Sum Query Under Limit (Prefix Sum)

## Problem

Given an integer array `nums`, a list of queries `queries` where `queries[i] = [x, y]`, and an integer `limit`, return a boolean array where each element is:

* `true` if the sum of the subarray `nums[x..y]` is **less than** `limit`,
* `false` otherwise.

**Example**

```
nums    = [1, 6, 3, 2, 7, 2]
queries = [[0, 3], [2, 5], [2, 4]]
limit   = 13
```

Subarray sums for the queries are `[12, 14, 12]`, so the answer is:

```
[true, false, true]
```

---

## Approach: Prefix Sum (Inclusive)

Build a prefix sum array `sums` where `sums[i]` is the sum of `nums[0..i]`.
Then any subarray sum `nums[l..r]` can be computed in `O(1)` as:

```
sum(l..r) = sums[r] - sums[l] + nums[l]
```

This avoids special-casing `l = 0` and lets us answer all queries in constant time after `O(n)` preprocessing.

* Time: `O(n + m)` where `n = nums.length` and `m = queries.length`
* Space: `O(n)` for the prefix array

> If values may be large, consider using `long` for the prefix sums to avoid integer overflow.

---

## Java Implementation

```java
import java.util.Arrays;

class Solution {
    public static boolean[] answerQueries(int[] nums, int[][] queries, int limit) {
        int[] sums = new int[nums.length];
        sums[0] = nums[0];
        for (int i = 1; i < nums.length; i++){
            sums[i] = sums[i - 1] + nums[i];
        }
        boolean[] answers = new boolean[queries.length];
        for (int i = 0; i < queries.length; i++){
            int left = queries[i][0];
            int right = queries[i][1];
            answers[i] = (sums[right] - sums[left] + nums[left]) < limit;
        }
        return answers;
    }

    public static void main(String[] args) {
        int[] nums = {1, 6, 3, 2, 7, 2};
        int[][] queries = {{0, 3}, {2, 5}, {2, 4}};
        int limit = 13;
        System.out.println(Arrays.toString(Solution.answerQueries(nums, queries, limit)));
    }
}
```

---

## Notes

* Alternative prefix style `(n+1)` is often cleaner: define `pref[0] = 0`, `pref[i+1] = pref[i] + nums[i]` and compute `sum(l..r) = pref[r+1] - pref[l]`.
* For safety with large sums, use `long[]` for prefix and `long` arithmetic.
