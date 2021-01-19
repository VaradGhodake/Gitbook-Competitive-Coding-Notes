### Sliding Window
https://leetcode.com/problems/minimum-window-substring/ <br />
We know the target dict. We can easily maintain current window with sliding window. <br />
For satisfy criteria, we need to know how many of the characters in target are in enough quantity. <br />
If we hit the target count for current character(not greater, that comes by default), we found a possible solution. <br />
Now contract the window (`start += 1`) until `found == required` character count
```py
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if not s or not t:
            return ''
        
        target, current = {}, {}
        for c in t:
            target[c] = target.get(c, 0) + 1
        
        start, end, min_len = 0, 0, float('inf')
        found, required = 0, len(target)
        result = ''
        
        while end < len(s):
            character = s[end]
            if character in target:
                current[character] = current.get(character, 0) + 1
                if current[character] == target[character]:
                    found += 1
            
            while found == required:
                if min_len > (end - start + 1):
                    min_len = (end - start + 1)
                    result = s[start: end+1]
                
                if s[start] in current:
                    current[s[start]] -= 1
                    if current[s[start]] < target[s[start]]:
                        found -= 1
                start += 1
                
            end += 1
        
        return result
```