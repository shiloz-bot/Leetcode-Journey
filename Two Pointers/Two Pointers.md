[[Easy] 125. Valid Palindrome](https://leetcode.cn/problems/valid-palindrome/)

- Time Complexity: O(n) n = length of s
- Space Complexity: O(1)
- Note: 
```
class Solution {
    public boolean isPalindrome(String s) {
        int leftIdx = 0;
        int rightIdx = s.length() - 1;
        while (leftIdx < rightIdx) {
            char leftChar = Character.toLowerCase(s.charAt(leftIdx));
            char rightChar = Character.toLowerCase(s.charAt(rightIdx));
            if (!Character.isLetterOrDigit(leftChar)) {
                leftIdx++;
                continue;
            }
            if (!Character.isLetterOrDigit(rightChar)) {
                rightIdx--;
                continue;
            }
            if (leftChar != rightChar) {
                return false;
            }
            leftIdx++;
            rightIdx--;
        }
        return true;
    }
}
```
