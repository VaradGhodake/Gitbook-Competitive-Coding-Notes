### Bucket Trick

This is inspired from bucket sort. Hashes are good for equalities but not for comparison or range operations, unless we want our search code to run in linear time.


This problem illustrates its use perfectly: <br />
We use sliding window + buckets here. `t` buckets to facilitate this. <br />
If two numbers are in the same bucket, we have found our solution. If they are in the neightboring buckets, they 'can' a possible solution.
https://leetcode.com/problems/contains-duplicate-iii/
```py
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        state = {}
        w = k + 1
        
        for i, n in enumerate(nums):
            b = n // (t + 1)
            
            # maintain sliding window
            if i >= w:
                del state[nums[i - w] // (t + 1)]
            
            if b in state:
                return True
            
            if b - 1 in state and abs(state[b - 1] - n) < (t + 1):
                return True
            
            if b + 1 in state and abs(state[b + 1] - n) < (t + 1):
                return True
            
            state[b] = n
        
        return False
```