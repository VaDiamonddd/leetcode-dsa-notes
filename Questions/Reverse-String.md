Write a function that reverses a string. The input string is given as an array of characters s.
You must do this by modifying the input array in-place with O(1) extra memory.
Constraints:
- 1 <= s.length <= 105
- s[i] is a printable ascii character.


**Input:** s = ["h","e","l","l","o"]

**Output:** = ["o","l","l","e","h]

```java
class Solution {
    public void reverseString(char[] s) {
        int i = 0;
        int j = s.length - 1;
        while (i <= (s.length - 1)/2){
            char copy = s[i];
            s[i] = s[j];
            s[j] = copy;
            i++;
            j--;
        }
    }
}
