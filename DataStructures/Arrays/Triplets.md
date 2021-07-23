### Triplets

https://leetcode.com/problems/count-number-of-teams/
```py
class Solution:
    def numTeams(self, rating: List[int]) -> int:
        s = [0 for i in range(0, len(rating))]
        g = [0 for i in range(0, len(rating))]
        total = 0

        for i in range(1, len(rating)):
            for j in range(0, i):
                if rating[i] > rating[j]:
                    total += s[j]
                    s[i] += 1

                if rating[i] < rating[j]:
                    total += g[j]
                    g[i] += 1

        return total
```
#### Optimization step by step

https://leetcode.com/problems/3sum/ <br />
This is modified 2 sum. We fix 2 elements and need to find the third one. <br />
Sets are used to avoid repetition. <br />
Ways to find the third one: <br />
1. Binary Search <br />
2. Save positions in a dictionary. Check the last position of that element, if that's greater than the second elem, we have found a possible set

```py
from collections import defaultdict

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        result_s = set()
        result = []
        positions = defaultdict(list)
        
        for i, num in enumerate(nums):
            positions[num].append(i)
        
        for i in range(0, n - 2):
            for j in range(i + 1, n - 1):
                sum_pos = positions[-(nums[i] + nums[j])]
                if sum_pos:
                    if sum_pos[-1] > j:
                        result_s.add(tuple(sorted([nums[i], nums[j], \
                                      -(nums[i] + nums[j])])))

        while result_s:
            x, y, z = result_s.pop()
            result.append([x, y, z])
        
        return result
```
Better approach with sorting
```py
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums = sorted(nums)
        n, target = len(nums), 0
        result = []
        i = 0
        
        while i < n-2:
            # avoid duplicate solutions; keep i value unique
            # do not enforce on the loop init value
            if i > 0 and nums[i] == nums[i-1]:
                i += 1
                continue
            
            j, k = i + 1, n-1
            
            while j < k:
                # avoid duplicate solutions; keep j value unique
                # do not enforce on the loop init value
                if j > (i+1) and nums[j] == nums[j-1]:
                    j += 1
                    continue
                
                tri_sum = nums[i] + nums[j] + nums[k]
                
                # target sum found
                if tri_sum == target:
                    result.append([nums[i], nums[j], nums[k]])
                    j += 1
                    k -= 1
                
                # sum smaller than target, increase j to increase tri_sum
                if tri_sum < target:
                    j += 1
                # sum greater than target, decrease j to decrease tri_sum
                elif tri_sum > target:
                    k -= 1
            
            i += 1
                
        return result
```