#### Combination sum and coin-change problems
Multiple combinations possible: <br />
All the possible ways to reach at the sum/amount: <br />
    Unique sets: https://leetcode.com/problems/combination-sum/ (Check `combinatorics` section) <br /> 
    Unique combinations: https://leetcode.com/problems/combination-sum-iv/ <br />
    Unique combinations (including unique values at indices as well): https://leetcode.com/problems/combination-sum-ii/ <br />
Find min steps to reach at the sum/amount: https://leetcode.com/problems/coin-change/ <br />
Find total number of ways to reach at the sum/amount https://leetcode.com/problems/coin-change-2/<br />
Different constraints: https://leetcode.com/problems/combination-sum-iii/ <br />

_There is no other way than backtracking for finding the actual sets_

* Recursive solution:
Rule of thumb: <br />

If you want to check the min number of steps necessary, recursive call should read: <br />
```py
min_steps = float('inf')
for j in range(i, len(coins)):
    min_steps = min(min_steps, 1 + backtrack(j, current + coins[j]))
```
Success cases should return `0` <br />
Failure cases (out of bounds): `(i == n or current > amount)` should return `float('inf')` <br /> 

If you want to know the total number of ways to reach at the amount/sum: <br />
```py
total_steps = 0
for j in range(i, len(coins)):
    total_steps += backtrack(j, current + coins[j])
```
Success cases should return `1` <br />
Failure cases (out of bounds): `(i == n or current > amount)` should return `0` <br /> 

Notice how `j` runs from `i` to `len(coins)`. This is if we don't want `(1, 6, 1)` again if `(1, 1, 6)` is already included

##### Top-down recursion/ memoization
This is an intuitive step <br />
Declare memo dict and store results

##### DP 
Think from the perspective of constant amount <br />
for all the coins, we want to take `min` or `add` <br />

```py
        for i in range(1, amount + 1):
            for coin in coins:
                if i < coin:
                    continue
                    
                dp[i] = min(dp[i], dp[i - coin] + 1)
```
For coin change 2, we need to run these loops in the reverse order as we have to eliminate duplicate combinations and we don't necessarily need to find a "complete" answer for a previous target.

Similar idea: <br />
https://leetcode.com/problems/word-break-ii/ 
```py
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        dp = [[] for i in range(0, len(s) + 1)]
        
        # check of there's any character in the 
        # string that's just 
        # not present in wordDict
        s_set = set(s)
        for word in wordDict:
            s_set -= set(word)
            
        if s_set:
            return []
        
        for i in range(0, len(dp)):
            for word in wordDict:
                if not s[:i].endswith(word):
                    continue
                
                if len(word) == i:
                    dp[i].append(word)
                    continue
                
                for match in dp[i - len(word)]:
                    dp[i].append(match + " " + word)
        
        return dp[-1]
```


https://leetcode.com/problems/knight-dialer/ <br />
We need to store current dp; which represents the state after given hop. <br />
For a given hop, we jump to the possible destinations; this value needs to be preserved as the updated one will change destination's calculations. So, we save that state and compute.
```py
class Solution:
    def knightDialer(self, N: int) -> int:
        dp = [1 for i in range(0, 10)]
        hops = [(4, 6), (6, 8), (7, 9), (4, 8), \
                (0, 3, 9), [], (0, 1, 7), (2, 6), (1, 3), (2, 4)]
        
        mod = 10 ** 9 + 7
        
        for hop in range(0, (N - 1)):
            dp1 = [0] * 10
            
            for pos in range(0, 10):
                for n in hops[pos]:
                    dp1[n] += (dp[pos] % mod)
            dp = dp1
            
        return sum(dp) % mod
```