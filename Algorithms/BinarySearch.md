### Binary Search

Iterative solutions are quicker than the recursive ones. <br />
Recursive are cleaner though <br />

It's necessary to understand where BS terminates. This can answer a lot of questions. It's the start when you want to check the appropriate place for a num in sorted array <br />
Use an auxillary variable to store the results at mid. Decide what direction we have to go in case of equality. Minimum of such solutions or maximum, etc.

Good problem set:
* https://leetcode.com/discuss/general-discussion/691825/binary-search-for-beginners-problems-patterns-sample-solutions

https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets
```py
class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        n = len(bloomDay)
        start = bloomDay[0]
        end = bloomDay[0]
        result = -1
        
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
            
        while start <= end:
            mid = start + (end - start) // 2
            
            if verify(mid):
                end = mid - 1
                result = mid
            else:
                start = mid + 1
                
        return result
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