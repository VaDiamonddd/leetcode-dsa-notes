# Task
Given a binary array `nums` and an integer `k`, return *the maximum number of consecutive `1`'s in the array if you can flip at most `k` `0`'s.*

**Example:**
> **Input:** nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2\
**Output:** 6\
**Explanation:** [1,1,1,0,0,**1**,1,1,1,1,**1**]\
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
>
**Constraints:**

`1 <= nums.length <= 105`\
    `nums[i] is either 0 or 1.`\
    `0 <= k <= nums.length`
# Thinking
## Attempt 1
At first, I decided to write the most inefficient code ever with 2 `while` loops. However, it didn't work at all: LeetCode could compile it, but the answer was wrong, and other IDEs couldn't do anything, just endless waiting.
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int right = 0;
        int best = 0;
        while(left < nums.length){
            int current = 0;
            int zeros = 0;
            while (zeros <= k && right < nums.length){
                if (nums[right] == 1){
                    current++;
                    right++;
                } else{
                    zeros++;
                    right++;
                    current++;
                }
            }
            if (current > best){
                best = current;
            }
            left++;
        }
        return best;
    }
}
```
