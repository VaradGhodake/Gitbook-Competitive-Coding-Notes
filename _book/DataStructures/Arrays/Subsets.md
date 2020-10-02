### Subsets

Geeks for Geeks archives: https://www.geeksforgeeks.org/tag/subset/ <br />

Optimization checks:
1. Sorting the whole array
2. Priority queues (check `Miscellaneous Problems` section)

For each element, there are 2 choices: Take or leave <br />
Two ways to implement this: <br />
1. Recursion
2. DP

#### Standard questions
https://www.geeksforgeeks.org/partition-problem-dp-18/
1. `isSubsetSum(list, n, sum / 2) = isSubsetSum(list, n - 1, sum / 2) or isSubsetSum(list, n - 1, sum / 2 - list[n - 1])`

2.  <br /> 
![partition-problem-dp](../../static/dp1.png)

#### DP 
Problems like: <br />
* LIS
* https://leetcode.com/problems/largest-divisible-subset/

Base DP method is LIS. <br /> 
Smart implementation: https://leetcode.com/problems/largest-divisible-subset/discuss/84006/Classic-DP-solution-similar-to-LIS-O(n2)

```py
class Solution:
    def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
        nums = sorted(nums)
        set_len = [1] * len(nums)
        pre = [-1] * len(nums)
        max_len = 0
        index = -1
        
        for i, target in enumerate(nums):
            for j in range(0, i):
                if target % nums[j] == 0:
                    if (1 + set_len[j]) > set_len[i]:
                        pre[i] = j
                        set_len[i] = 1 + set_len[j]
            
            if set_len[i] > max_len:
                index = i
                max_len = set_len[i]
        
        result = []
        while index != -1:
            result.append(nums[index])
            index = pre[index]
            
        return result
```