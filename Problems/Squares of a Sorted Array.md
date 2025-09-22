## Task
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

**Example:**
```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```
Constraints:
- 1 <= nums.length <= 104
- -104 <= nums[i] <= 104
- nums is sorted in non-decreasing order.

## Logic
The squares of the left and rightmost parts of the array will always be the largest ones, since their absolute values are the largest ones in a sequence.
Therefore, if I compare the squares using two pointers and put the larger one into a new array, moving the larger square's index, I will get a sorted array of squares.

## My path
At first, I wrote the code that didn't account for the case when i and j were equal. My program created an out-of-bounds error because after it thought that the right and left were equal, it put the right into the first position of the array, and the left one (same number) into out of bounds. So, I created a special case for when i and j are equal and everything is fine.

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int i = 0;
        int j = nums.length - 1;
        int[] solution = new int[nums.length];
        int cout = nums.length - 1;
        while (i <= j){
            int left = nums[i]*nums[i];
            int right = nums[j]*nums[j];
            if(i == j){
                solution[cout] = right;
                i++;
                j--;
            } else if (right > left){
                solution[cout] = right;
                cout--;
                j--;
            } else if(left > right){
                solution[cout] = left;
                cout--;
                i++;
            } else if(left == right){
                solution[cout] = left;
                solution[cout - 1] = right;
                cout -= 2;
                i++;
                j--;
            }
        }
        return solution;
    }
}
