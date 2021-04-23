### Binary Search


Good article: https://leetcode.com/discuss/general-discussion/786126/python-powerful-ultimate-binary-search-template-solved-many-problems

Iterative solutions are quicker than the recursive ones. <br />

Monotonous function questions are interesting. You have to find the optimal value of a variable between min and max, not an array eg: bouquet and boat capacity questions below

It's necessary to understand where BS terminates. This can answer a lot of questions. It's the start when you want to check the appropriate place for a num in sorted array <br />
Decide what direction we have to go in case of equality. Minimum of such solutions or maximum, etc <br />

One of these things should be done: <br />
_Remember:_ we should never have a case where `start <= end` and somehow start or end do not change.
1. (`start < end` loop) include `mid` in the next iteration. i.e. use `end = mid`; *DO NOT do the same thing for start*. Try to find the element _after_ pivot. [Look at https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/]
2. (`start <= end` loop) somehow get the equality thing sorted; decrement end or start: LeetCode 154

Good problem set:
* https://leetcode.com/discuss/general-discussion/691825/binary-search-for-beginners-problems-patterns-sample-solutions

https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
```py
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums or nums[-1] < target:
            return [-1, -1]
        
        def binarySearch(k, start=0, end=len(nums)):
            if nums[-1] < k:
                return len(nums)
            
            while start < end:
                mid = start + (end - start) // 2
                
                if nums[mid] < k:
                    start = mid + 1
                else:
                    end = mid
            
            return start
        
        target_idx = binarySearch(target)
        if nums[target_idx] != target:
            return [-1, -1]
        
        target_gt_idx = binarySearch(target + 1)
        return [target_idx, target_gt_idx-1]
```
https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets
```py
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        n = len(bloomDay)
        start = bloomDay[0]
        end = bloomDay[0]
        
        if m * k > n:
            return -1
        
        for day in bloomDay:
            start = min(start, day)
            end = max(end, day)
            
        def verify(mid):
            streak = 0
            req = m
            
            for i, day in enumerate(bloomDay):
                if mid >= day:
                    streak += 1
                
                if mid < day or i == (n - 1):
                    req -= (streak // k)
                    streak = 0
                    
                    if req <= 0:
                        return True
                    
            return False
            
        while start < end:
            mid = start + (end - start) // 2
            
            if verify(mid):
                end = mid
            else:
                start = mid + 1
                
        return start
```
https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/ <br />
min = max from array <br />
max = sum of array <br />
`can_ship` function is crucial
```py
class Solution:
    def shipWithinDays(self, weights: List[int], D: int) -> int:
        max_w = float('-inf')
        sum_w = 0
        for w in weights:
            sum_w += w
            max_w = max(max_w, w)
        
        if D == 1:
            return sum_w
        
        if D == len(weights):
            return max_w
        
        def can_ship(w):
            days_left = D - 1
            current = weights[0]
            
            for wg in weights[1:]:
                if current + wg <= w:
                    current += wg
                else:
                    current = wg
                    days_left -= 1
                    if days_left < 0:
                        return False
            return True
        
        start = max_w
        end = sum_w
        while start < end:
            mid = start + (end - start) // 2
            able = can_ship(mid)
            
            if able:
                end = mid
            else:
                start = mid + 1
        
        return start
```
https://leetcode.com/problems/sum-of-mutated-array-closest-to-target/
```py
class Solution:
    def findBestValue(self, arr: List[int], target: int) -> int:
        start = 0
        end = arr[0]
        diff = float('inf')
        result = float('inf')
        
        for num in arr:
            end = max(end, num)
            
        def calc_sum(mid):
            return sum(i if i < mid else mid for i in arr)
        
        while start <= end:
            mid = start + (end - start) // 2
            
            current_diff = calc_sum(mid) - target
            
            if abs(current_diff) < diff:
                result = mid
                diff = abs(current_diff)
                
            if abs(current_diff) == diff:
                result = min(result, mid)
            
            if current_diff >= 0:
                end = mid - 1
            else:
                start = mid + 1
                
        return result
```
https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:        
        def find_first_greater(start, end, target):
            while start < end:
                mid = start + (end - start) // 2
                
                if preorder[mid] > target:
                    end = mid
                else:
                    start = mid+1
            return start
        
        def constructTree(start, end):
            if start > end:
                return None
            
            current = TreeNode(preorder[start])
            fg = find_first_greater(start + 1, end, preorder[start])
            
            # there is no greater element
            # this step is important
            if fg >= len(preorder) or preorder[fg] <= preorder[start]:
                fg = end + 1
            
            current.left = constructTree(start + 1, fg - 1)
            current.right = constructTree(fg, end)
            return current
            
        return constructTree(0, len(preorder)-1)
```

https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
```py
class Solution:
    def findMin(self, nums: List[int]) -> int:
        start = 0
        end = len(nums) - 1
        
        while start < end:
            mid = start + (end - start) // 2
            
            if nums[mid] <= nums[end]:
                end = mid
            
            if nums[mid] > nums[end]:
                start = mid + 1
        
        return nums[start]
```
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/ <br />
Unlike the (i) problem, in case of equality, we need to reduce the end by 1.
```py
class Solution:
    def findMin(self, nums: List[int]) -> int:
        start = 0
        end = len(nums) - 1
        result = nums[0]
        
        if nums[start] < nums[end]:
            return nums[start]
        
        while start <= end:
            mid = start + (end - start) // 2
            
            if nums[mid] < nums[end]:
                end = mid
            elif nums[mid] > nums[end]:
                start = mid + 1
            else:
                end -= 1
            
        return nums[start]
```
https://leetcode.com/problems/sqrtx/ <br />
Binary Search will take us to the closest point of the answer
```py
class Solution:
    def mySqrt(self, x: int) -> int:
        start = 0
        end = x
        
        while start <= end:
            mid = start + (end - start) // 2
            
            mid_sq = mid * mid
            if mid_sq == x:
                return mid
            
            if mid_sq > x:
                end = mid - 1
            else:
                start = mid + 1
        
        return start if start * start < x else start - 1
```