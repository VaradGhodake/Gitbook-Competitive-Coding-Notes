### Palindrome

https://leetcode.com/problems/longest-palindromic-substring/ <br />
Expand from the center. <br /> 
For every index, 2 palindromes possible: <br />
1. That character being a center (odd length) <br />
2. That character being a left center (even length) <br />
Now, write a util function and expand with `l` and `r` until string end and as 
long as the characters match. <br />
`max` function also supports `key` like `sorted` function
```py
class Solution:
    def longestPalindrome(self, s: str) -> str:
        result = ""
        
        def find_len(l, r):
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1
            
            return l+1, r-1
        
        for i in range(len(s)):
            # odd length palindromes
            o_start, o_end = find_len(i, i)
            
            # even length palindromes
            e_start, e_end = find_len(i, i+1)
            result = max(result, s[o_start:o_end+1], s[e_start:e_end+1], key=lambda x: len(x))
        
        return result
```

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
Solution using start and end
```py
class Solution:
    def validPalindrome(self, s: str) -> bool:
        left, right = 0, len(s)-1
        
        def isPalindrome(start, end):
            while start < end:
                if s[start] != s[end]:
                    return False
                start += 1
                end -= 1
                
            return True
        
        while left < right:
            if s[left] == s[right]:
                left += 1
                right -= 1
            else:
                return isPalindrome(left+1, right) or isPalindrome(left, right-1)
        
        return True
```