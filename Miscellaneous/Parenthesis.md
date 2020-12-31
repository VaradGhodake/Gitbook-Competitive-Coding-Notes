### Parenthesis questions

* https://leetcode.com/problems/valid-parentheses/
* https://leetcode.com/problems/generate-parentheses/
* https://leetcode.com/problems/valid-parenthesis-string/
* https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
* https://leetcode.com/problems/remove-outermost-parentheses/

#### Valid parenthesis <br />
Simple idea. Push to the stack if it's an open one; pop if it's a matching closing one. <br />
If the stack is empty and we encounter a closed one, that's an instant `False`

#### Generate parentheses
Keep a count of open and closed ones. <br />
Success cases: string length 2 * n and stack == 0 <br />
Failure cases: string length 2 * n and stack != 0 OR stack < 0 OR stack > n <br />
We just backtrack:
```py
            backtrack(current + '(', stack + 1)
            backtrack(current + ')', stack - 1)
```

#### Valid parenthesis string
Backtrack different possibilities <br />
'*' comes in every backtrack call based on character <br />
Top-down backtrack is very intuitive
```py

            memo[(i, stack)] = False
            
            if s[i] in ['(', '*']:
                memo[(i, stack)] = memo[(i, stack)] or \
                                   backtrack(i + 1, stack + 1)
            
            if s[i] == '*':
                memo[(i, stack)] = memo[(i, stack)] or \
                                   backtrack(i + 1, stack)
            
            if s[i] in [')', '*']:
                if not stack:
                    return memo[(i, stack)]

                memo[(i, stack)] = memo[(i, stack)] or \
                                   backtrack(i + 1, stack - 1)
```
Tricky: https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/ <br />
If it's a normal character, extend current string <br />
If it's a `(`, push the whole thing on stack and make current an empty string <br />
If it's a `)`, `pop one from the stack + ( + current + )`
Return `return ''.join(stack) + current`
```py
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        stack = []
        current = ''
        
        for c in s:
            if c == '(':
                stack.append(current)
                current = ''
            elif c == ')':
                if stack:
                    current = stack.pop() + '(' + current + ')'
            else:
                current += c
                
        return ''.join(stack) + current
```
https://leetcode.com/problems/remove-outermost-parentheses/
```py
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        stack = []
        start = 0
        result = ""
        
        for i, c in enumerate(S):
            if not stack:
                start = i

            if c == '(':
                stack.append(c)
            else:
                stack.pop()
            
            if not stack:
                result += S[start + 1: i]
        
        return result            
```