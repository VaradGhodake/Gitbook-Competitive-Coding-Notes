### Stacks and Queues

#### Subtle tricks to be used:
Stacks: <br />
https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/
```py
class Solution:
    def removeDuplicates(self, S: str) -> str:
        stack = []
        LAST = -1
        
        for i, s in enumerate(S):
            if stack and stack[LAST] == s:
                stack.pop()
            else:
                stack.append(s)
        
        return ''.join(stack)
```