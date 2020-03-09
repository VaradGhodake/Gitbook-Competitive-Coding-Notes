### Dynamic Programming

Dynamic programming is a technique that combines the correctness of complete search and the efficiency of greedy algorithms. Dynamic programming can be applied if the problem can be divided into overlapping subproblems that can be solved independently.

There are two uses for dynamic programming:
* Finding an optimal solution: We want to find a solution that is as large as possible or as small as possible.
* Counting the number of solutions: We want to calculate the total number of possible solutions.

#### Optimal Solution
eg. 0/1 Knapsack, partitions: exact sum, minimum sum difference

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

For exact sum subarray and minimum difference subarrays: <br />
`True` or `False` for each cell. Use or instead of `max(include, exclude)`
Need to have another loop over the table to find the 'pivot'
For minimum difference, find a True cell and that causes the least difference 
`(total_sum - 2 * cell_column)`

For the minimum number of squares required to get the sum, similar to the exact sum subarray. Rather than `True` or `False`, maintain the number of squares required for the sum

#### Number of solutions
eg. Climbing stairs (LeetCode 70)

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