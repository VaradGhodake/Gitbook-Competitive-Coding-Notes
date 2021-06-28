### Dynamic Programming IV

Two properties of DP problems: <br />
* Optimal substructure 
* Overlapping subproblems

This about how can we reach to the current positions from different other places. <br />
Need to use and auxillary array sometimes (knight dialer) <br />
Otherwise, if it depends only on the past results, just the current one if enough. 

https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/543/week-5-june-29th-june-30th/3375/
```py
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        X = n
        Y = m
        
        dp = [[0 for y in range(Y)] for x in range(X)]
        dp[0][0] = 1
        
        for x in range(X):
            for y in range(Y):
                if x == 0 and y == 0:
                    continue
                
                if x == 0:
                    dp[x][y] = dp[x][y - 1]
                    continue
                
                if y == 0:
                    dp[x][y] = dp[x - 1][y]
                    continue
                    
                dp[x][y] = dp[x - 1][y] + dp[x][y - 1]
                
        return dp[-1][-1]
```