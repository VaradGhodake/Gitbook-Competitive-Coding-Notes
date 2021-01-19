### Palindrome
https://leetcode.com/problems/valid-palindrome-ii/ <br />
Check characters one by one, if they are same, adjust `start` and `end` <br />
If they mismatch, direct check with string slices
```py
class Solution:
    def validPalindrome(self, s: str) -> bool:
        start, end = 0, len(s)-1
        
        while start <= end:
            if s[start] == s[end]:
                start += 1
                end -= 1
                continue
            
            if s[start+1:end+1] == s[start+1:end+1][::-1]:
                return True
            elif s[start:end] == s[start:end][::-1]:
                return True
            
            return False
        
        return True
```