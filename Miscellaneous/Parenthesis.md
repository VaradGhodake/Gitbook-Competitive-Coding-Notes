### Parenthesis questions

* https://leetcode.com/problems/valid-parentheses/
* https://leetcode.com/problems/generate-parentheses/
* https://leetcode.com/problems/valid-parenthesis-string/
* https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
* https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/
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
https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/ <br />
The ones that need to be removed are:
1. close brakets that come when there was no open one before it 
2. open ones that don't have a close one after them
Just need to find them and replace with an empty string.
Set is interchangable with list here because we don't care what open bracket a close one matches with; we'd just pop
```py
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        eq = list(s)
        opened = set()
        
        for i, c in enumerate(eq):
            if c == '(':
                opened.add(i)
            elif c == ')':
                if not opened:
                    eq[i] = ''
                else:
                    opened.pop()

        return ''.join([c if (i not in opened) else '' for i, c in enumerate(eq)])
```
https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/ <br />
A closed bracket without a corresponding opening one, can never to balanced afterwards, irrespective of what comes after <br />
An open bracket can be matched by a closed one immediately.
```py
class Solution:
    def minAddToMakeValid(self, S: str) -> int:
        open_brackets = 0
        closed_brackets = 0
        
        for i, s in enumerate(S):
            if s == '(':
                open_brackets += 1
            else:
                if not open_brackets:
                    closed_brackets += 1
                else:
                    open_brackets -= 1
        
        return open_brackets + closed_brackets
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
https://leetcode.com/problems/remove-invalid-parentheses/ <br />
This is a combination of two problems: first identify the number of removals required and then generate strings with backtracking <br />
`net` is basically `number of open brackets - number of closed brackets` <br />
Then it is just a matter of pruning the tree and finding the optimal solution
```py
class Solution:
    def removeInvalidParentheses(self, s: str) -> List[str]:
        net, removal = 0, 0
        result = set()
        visited = set()
        
        for c in s:
            # find the minimum number of removals to be made
            if c == '(':
                net += 1
            elif c == ')':
                net -= 1
            
            if net == -1:
                net = 0
                removal += 1
        removal += net
        
        def construct(index, net, string, removals):
            if (string, removals) in visited:
                return 
            else:
                visited.add((string, removals))
            
            if removals == -1:
                return
            
            if index == len(s):
                # we found the solution
                if net == 0 and removals == 0:
                    result.add(string)
                return
            
            if s[index] == '(':
                # include open bracket
                construct(index + 1, net + 1, string + '(', removals)
            
            if s[index] == ')':
                # include 
                if net > 0:
                    construct(index + 1, net - 1, string + ')', removals)
            
            if s[index] in ['(', ')']:
                # exclude brackets
                construct(index + 1, net, string, removals - 1)
            else:
                # normal character
                construct(index + 1, net, string + s[index], removals)
        
        construct(0, 0, '', removal)
        return result
```
https://leetcode.com/problems/longest-valid-parentheses/ <br />
start from this state: `[-1]` <br />
Only unbalanced state will empty the stack, push that index to be the boundary <br />
```py
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        stack = [-1]
        max_len = 0
        
        for i, c in enumerate(s):
            if c == '(':
                stack.append(i)
                continue
            
            stack.pop()
            if stack:
                max_len = max(max_len, i - stack[-1])
            else:
                stack.append(i)
        
        return max_len
```