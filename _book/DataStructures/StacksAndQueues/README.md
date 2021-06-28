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
We can't pop anything that hasn't been pushed. If the top of the stack matches `popped[k]`, we have to pop right away because if we don't and push something, there's no way we can pop that element again. Why? _hint:_ the elements are distinct. <br />
So the `while` base case just becomes if `k < n` and we can return `False` if we reach at the end of `pushed` and the top doesn't match `popped[k]` or there's nothing to push
```py
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        j = k = 0
        n = len(pushed)
        stack = []
        
        while k < n:
            if not stack:
                if j == n:
                    return False
                
                stack.append(pushed[j])
                j += 1
                continue
                
            if popped[k] == stack[-1]:
                stack.pop()
                k += 1
            else:
                if j == n:
                    return False
                
                stack.append(pushed[j])
                j += 1
        
        return True
```