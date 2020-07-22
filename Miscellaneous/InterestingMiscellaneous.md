#### Need to find nth smallest or largest
Use max or min-heap or partial sort: quicksort variation <br />
A better way to achieve this is to create a heap of size `k` (target) first. For finding max k, use min-heap and vice-versa. <br />
https://leetcode.com/explore/challenge/card/july-leetcoding-challenge/546/week-3-july-15th-july-21st/3393/

LRU cache:  <br /> https://leetcode.com/problems/lru-cache/ <br />
Simple dict py-3

#### Priority queues
*1383:* https://leetcode.com/contest/weekly-contest-180/problems/maximum-performance-of-a-team/ <br />
*Solution:* https://leetcode.com/problems/maximum-performance-of-a-team/discuss/539797/C%2B%2BPython-Priority-Queue

*857:* https://leetcode.com/problems/minimum-cost-to-hire-k-workers/

#### Sort colors
https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/540/week-2-june-8th-june-14th/3357/ <br />
The trick is to maintain all-red pointer on its left, all-blue on its right and an iterator to just swap with red one if it's red and same for blue. Stop if you encounter the blue one.

Similar <br />
https://leetcode.com/problems/move-zeroes/submissions/
```py
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        i = 0
        non = 0
        
        while i < n:
            if nums[i]:
                nums[non], nums[i] = nums[i], nums[non]
                non += 1
            i += 1
            
        return nums
```