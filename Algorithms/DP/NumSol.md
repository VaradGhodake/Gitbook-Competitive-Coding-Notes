### Number of solutions

The key here is to add dp values of previous computations instead of doing `1 + min(dp[prev1], dp[prev2])` for other ones.

* https://leetcode.com/problems/unique-paths-ii/
```py
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        X = len(obstacleGrid)
        Y = len(obstacleGrid[0])
        
        dp = [[0 for _ in range(Y)] for _ in range(X)]
        dp[0][0] = 1
        
        for x in range(X):
            for y in range(Y):
                if obstacleGrid[x][y] == 1:
                    dp[x][y] = 0
                    continue
                
                if x > 0:
                    dp[x][y] += dp[x-1][y]
                
                if y > 0:
                    dp[x][y] += dp[x][y-1]
        
        return dp[-1][-1]
```