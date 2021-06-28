### 2 Pointers technique

https://leetcode.com/problems/two-sum/<br />
https://leetcode.com/problems/3sum/ <br/>
https://leetcode.com/problems/boats-to-save-people/

Two pointers is really an easy and effective technique which is typically used for searching pairs in an array.
 Two pointers:
At either ends and decide which one to move
One faster and one slower
Maintain subarray size of K’s props

##### General solution structure: <br />
https://leetcode.com/problems/container-with-most-water/ <br />
Container height is limited by smaller one, move that forward
```py
class Solution:
    def maxArea(self, height: List[int]) -> int:
        l, r = 0, len(height)-1
        water = 0
        
        while l < r:
            water = max(water, (r-l) * min(height[l], height[r]))
            
            if height[r] < height[l]:
                r -= 1
            else:
                l += 1
        
        return water
```
https://leetcode.com/explore/challenge/card/30-day-leetcoding-challenge/528/week-1/3286/

```py
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        n = len(nums)
        z = 0
        p = 0
            
        while (not p == (n - 1)):
            while z < n and nums[z]:
                z += 1 
            
            p = z + 1

            while p < n and (not nums[p]):
                p += 1
            
            if p >= n or z >= n:
                break

            nums[z], nums[p] = nums[p], nums[z]
            
        return nums
```