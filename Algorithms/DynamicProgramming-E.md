### Step-by-step solved DP problems
##### Essential leetcode questions for practice:
Decode Ways <br />
Word Break <br />
Perfect Squares <br />
Coin-change <br />
LIS <br />
LCS <br /> 
_Decent problemset:_ https://blog.usejournal.com/top-50-dynamic-programming-practice-problems-4208fed71aa3 <br />
Visualize the overlap with the recursion tree diagram given on the "Solution explaination" pages. 

#### Combination sum and coin-change problems
Multiple combinations possible: <br />
All the possible ways to reach at the sum/amount: <br />
    Unique sets: https://leetcode.com/problems/combination-sum/ <br />
    Unique combinations: https://leetcode.com/problems/combination-sum-iv/ <br />
    Unique combinations (including unique values at indices as well): https://leetcode.com/problems/combination-sum-ii/ <br />
Find min steps to reach at the sum/amount: https://leetcode.com/problems/coin-change/ <br />
Find total number of ways to reach at the sum/amount https://leetcode.com/problems/coin-change-2/<br />
Different constraints: https://leetcode.com/problems/combination-sum-iii/

* Recursive solution:
Rule of thumb: <br />
If you want to check the min number of steps necessary, recursive call should read: <br />
```py
min_steps = float('inf')
for j in range(i, len(coins)):
    min_steps = min(min_steps, 1 + backtrack(j, current + coins[j]))
```
Success cases should return `0`
Failure cases (out of bounds): `(i == n or current > amount)` should return `float('inf')` <br /> 

If you want to read the total number of ways to reach at the amount/sum: <br />
```py
total_steps = 0
for j in range(i, len(coins)):
    total_steps += backtrack(j, current + coins[j])
```
Success cases should return `1`
Failure cases (out of bounds): `(i == n or current > amount)` should return `0` <br /> 

Notice how `j` runs from `i` to `len(coins)`. This is if we don't want `(1, 6, 1)` again if `(1, 1, 6)` is already included

##### Top-down recursion/ memoization
This is an intuitive step <br />
Declare memo dict and store results

##### DP 
Think from the perspective of constant amount <br />
for all the coins, we want to take `min` or `add` <br />

```py
        for coin in coins:
            for i in range(1, amount + 1):
                if i < coin:
                    continue
                    
                dp[i] += dp[i - coin]
```

```py
        for coin in coins:
            for i in range(1, amount + 1):
                if i < coin:
                    continue
                    
                dp[i] = min(dp[i], dp[i - coin])
```
For combination-iv, we need to run these loops in the reverse order

#### Longest Common Subsequence
https://leetcode.com/problems/longest-common-subsequence/ <br />
Solution explaination: https://www.techiedelight.com/longest-common-subsequence/ <br />

##### Recursion:
```py
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        def LCS(m, n):
            if not m or not n:
                return 0
            
            if text1[m - 1] == text2[n - 1]:
                return LCS(m - 1, n - 1) + 1
                
            return max(LCS(m - 1, n), \
                       LCS(m, n - 1))
        
        return LCS(len(text1), len(text2))
```

##### Recursion + Memoization
```py
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        memo = dict()
        
        def LCS(m, n):
            if (m, n) in memo:
                return memo[(m, n)]
            
            if not m or not n:
                memo[(m, n)] = 0
                return memo[(m, n)]
            
            if text1[m - 1] == text2[n - 1]:
                memo[(m, n)] = LCS(m - 1, n - 1) + 1
            else:
                memo[(m, n)] =  max(LCS(m - 1, n), \
                                    LCS(m, n - 1))
            
            return memo[(m, n)]
        
        return LCS(len(text1), len(text2))
```

##### DP
```py
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        X = len(text2)
        Y = len(text1)
        
        dp = [[0 if x == 0 or y == 0 else -1 for y in range(0, Y + 1)] \
              for x in range(0, X + 1)]
        
        for x in range(1, X + 1):
            for y in range(1, Y + 1):
                if text1[y - 1] == text2[x - 1]:
                    dp[x][y] = 1 + dp[x - 1][y - 1]
                else:
                    dp[x][y] = max(dp[x - 1][y], dp[x][y - 1])
        
        return dp[X][Y]
```

#### Longest Palindromic Subsequence
https://leetcode.com/problems/longest-palindromic-subsequence/ <br />
Solution explaination: https://www.techiedelight.com/longest-palindromic-subsequence-using-dynamic-programming/ <br />

##### Recursion:
```py
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        
        def LPS(m, n):
            if m > n:
                return 0
            
            if m == n:
                return 1
            
            if s[m] == s[n]:
                return LPS(m + 1, n - 1) + 2
            else:
                return max(LPS(m + 1, n), \
                           LPS(m, n - 1))
            
        return LPS(0, n - 1)
```

##### Recursion + Memoization
```py
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        n = len(s)
        memo = dict()
        
        def LPS(m, n):
            if m > n:
                return 0
            
            if (m, n) in memo:
                return memo[(m, n)]
            
            if m == n:
                memo[(m, n)] = 1
                return memo[(m, n)]
            
            if s[m] == s[n]:
                memo[(m, n)] = LPS(m + 1, n - 1) + 2
            else:
                memo[(m, n)] = max(LPS(m + 1, n), \
                                   LPS(m, n - 1))
            
            return memo[(m, n)]
            
        return LPS(0, n - 1)
```