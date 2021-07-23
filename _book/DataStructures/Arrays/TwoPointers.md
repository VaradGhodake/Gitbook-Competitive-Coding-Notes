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
#### Partitioning

1. Select last element as a `pivot` and first element as the `left_idx`
2. Add a `runner` to loop from left to (right-1)
3. `left_idx`to keep smaller elements at the correct places. Swap (left_idx <-> runner elems) if runner pointed elem is smaller than the pivot
4. Swap `left_idx` and `pivot_idx` to put pivot in the right place
```py
        def partition(_left, _right):
            left_idx, pivot_idx = _left, _right
            
            for runner in range(_left, _right):
                if nums[runner] < nums[pivot_idx]:
                    nums[left_idx], nums[runner] = nums[runner], nums[left_idx]
                    left_idx += 1
                
            
            nums[pivot_idx], nums[left_idx] = nums[left_idx], nums[pivot_idx]
            return left_idx
```

https://leetcode.com/problems/move-zeroes/
```py
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        left_idx = runner = 0
        n = len(nums)
        
        while runner < n:
            if nums[runner] != 0:
                nums[left_idx], nums[runner] = nums[runner], nums[left_idx]
                left_idx += 1
            
            runner += 1
        
        return 0
```
https://leetcode.com/problems/remove-duplicates-from-sorted-array/
```py
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        left_idx, runner = 1, 1
        n = len(nums)
        
        while runner < n:
            if nums[runner] != nums[runner-1]:
                nums[left_idx] = nums[runner]
                left_idx += 1
            
            runner += 1
        
        return left_idx
```
