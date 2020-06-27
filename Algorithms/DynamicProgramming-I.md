### Dynamic Programming

Dynamic programming is a technique that combines the correctness of complete search and the efficiency of greedy algorithms. Dynamic programming can be applied if the problem can be divided into overlapping subproblems that can be solved independently.

We should always go: `recursive solution` -> `memoization` -> `Dynamic Programming` <br />
There are a lot of memoization solved problems in leetcode -> Top interview questions -> DP <br />
Memoization basically insures that we solve a particular subproblem only once to improve runtime

Recursive solution: For problems with structure of `take or leave`, check how it depends on previous values and recurse conditionally: make except and include cases and take max or min. eg. coin-change, LIS, perfect squares, word break, etc  
#### Half the problem is solved once you figure out that the problem is a DP one

Template: https://leetcode.com/discuss/general-discussion/651719/how-to-solve-dp-string-template-and-4-steps-to-be-followed

There are two uses for dynamic programming:
* Finding an optimal solution: We want to find a solution that is as large as possible or as small as possible.
* Counting the number of solutions: We want to calculate the total number of possible solutions.

#### 1. Optimal Solution
eg. 0/1 Knapsack, partitions: exact sum, minimum sum difference, leetcode 1035

Knapsack program:

```py
class Solution:
    def knapsack(self, weights, values, bag_weight):
        table = [[0] * (bag_weight + 1)] for _ in (len(values) + 1)]
                   
        for i in range(0, len(values)+ 1):
            for weight in range(0, bag_weight + 1):
                if(i == 0 or weight == 0):
                    table[i][weight] = 0
			    elif(weight < weights[i - 1]):
                    table[i][weight] = max(
                    weights[i - 1] + table[i - 1][w - weights[w - 1]],
                    table[i - 1][w]
                    )
                else:
                    table[i][weight] = table[i - 1][weight]

         return table[len(values)][bag_weight]
```

##### Similar problem for a 1D array
https://leetcode.com/problems/house-robber/ <br />

Recursive solution is intuitive
```py
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n == 0:
            return 0
        if n == 1:
            return nums[0]
        
        def recurse(current: int, i: int) -> int:
            if i >= n:
                return current
            
            return max(
                recurse(current, i + 1),
                recurse(current + nums[i], i + 2)
            )
            
        return recurse(0, 0)            
```

Solution using DP:
```py
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        
        if n == 0:
            return 0
        if n == 1:
            return nums[0]

        memo = [0] * n
        memo[0] = nums[0]
        memo[1] = max(nums[0], nums[1])
        
        for i in range(2, n):
            memo[i] = max(memo[i - 1], memo[i - 2] + nums[i])
        
        return memo[n - 1]
```

For exact sum subsets and minimum difference subsets: <br />
`True` or `False` for each cell. Use or instead of `max(include, exclude)`
Need to have another loop over the table to find the 'pivot'
For minimum difference, find a True cell and that causes the least difference 
`(total_sum - 2 * cell_column)`

For the minimum number of squares required to get the sum, similar to the exact sum subarray. Rather than `True` or `False`, maintain the number of squares required for the sum

#### 2. Number of solutions
eg. Climbing stairs (LeetCode 70), unique-paths <br />
https://leetcode.com/problems/unique-paths/submissions/ <br />

Recursive solution: <br />
```py
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        X = n
        Y = m
        visited = [[False for i in range(0, Y)] for j in range(0, X)]

        def dfs(x: int, y: int) -> int:
            if x < 0 or x >= X or y < 0 or y >= Y:
                return 0
            
            if visited[x][y]:
                return 0
            
            if x == (X - 1) and y == (Y - 1):
                return 1
            
            visited[x][y] = True
               
            total = dfs(x + 1, y) + \
                    dfs(x, y + 1) 
            
            visited[x][y] = False
            
            return total
        
        return dfs(0, 0)
```
DP Solution: <br />
```py
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        X = n
        Y = m
        
        if X == 0 and Y == 0:
            return 0
        
        if X == 1 and Y == 1:
            return 1
        
        if X == 1:
            return 1
        
        if Y == 1:
            return 1
        
        memo = [[0 for y in range(0, Y)] for x in range(0, X)]
        
        for x in range(0, X):
            for y in range(0, Y):
                if x == 0 and y == 0:
                    memo[x][y] = 1
                    continue
                
                if x == 0 or y == 0:
                    memo[x][y] = 1
                    continue
                    
                memo[x][y] = memo[x - 1][y] + memo[x][y - 1]
                
        return memo[X - 1][Y - 1]
```
Unique paths II is similar but we set count to `0` in case there is an obstruction. <br />
For row and column cells, `row[i] = row[i - 1]` and similar to columns <br />
Rest is similar

##### Similar problem for a 1D array

Climbing stairs: <br />
For each step, there are multiple choices, generally 2. <br />
Brute force solution goes something like this:

```py
class Solution:
    def solution_function(self, n):
        if(success_base_case):
	       return 1
        if(failure_base_case):
	       return 0

    return self.solution_function(case_one, n) + self.solution_function(case_two, n)
```
Time Complexity: O(2^n) - tree size

Check if the problem satisfies Optimal Substructure Property <br />
Find an equation for the answer <br />
Here,
```py
dp[n] = dp[n  -  1] + dp[n - 2] #(climb stairs case)

dp = [0] * (n + 1)
dp[0] = 0
dp[1] = 1
dp[2] = 2

for i in range(3, n):
            dp[i] = dp[i - 1] + dp[i - 2]
return dp[n]
```
From Leetcode:
>Usually, solving and fully understanding a dynamic programming problem is a 4 step process: <br />
>1. Start with the recursive backtracking solution
>2. Optimize by using a memoization table (top-down dynamic programming)
>3. Remove the need for recursion (bottom-up dynamic programming)
>4. Apply final tricks to reduce the time / memory complexity