### Interval problems

Template for simple questions like: <br />
* 435 Non-overlapping Intervals 
* 56 Merge Intervals 
* 252 Meeting Rooms
* 253 Meeting Rooms II 
* https://leetcode.com/problems/interval-list-intersections/solution/ <br />

For trivial questions, we sort based on the starting time. <br />
https://leetcode.com/problems/non-overlapping-intervals/
```py
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals = sorted(intervals, key = lambda x: x[0])
        min_end = float('-inf')
        removed = 0
        
        for start, end in intervals:
            if start >= min_end:
                min_end = end 
            else:
                removed += 1
                min_end = min(min_end, end)
        
        return removed
```
https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/
```py
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        intervals = sorted(points, key = lambda x: x[0])
        min_end = float('-inf')
        arrows = 0
        
        for start, end in intervals:
            if start <= min_end:
                min_end = min(min_end, end)    
            else:
                min_end = end
                arrows += 1

        return arrows
```
https://leetcode.com/problems/merge-intervals/ <br />
In case of overlap, stretch the previous one (last one in the intervals list); do not push
```py
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return intervals
        
        intervals = sorted(intervals, key = lambda x: x[0])
        result = []
        
        for start, end in intervals:
            if not result or (start > result[-1][1]):
                result.append([start, end])
            else:
                result[-1][1] = max(result[-1][1], end)
                
        return result
```

https://leetcode.com/problems/insert-interval/ <br />
Very similar to merge intervals. In case of overlap, do not push to the result; stretch the newInterval
```py
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        result = []
        n_s, n_e = newInterval
        pushed = False
        
        for s, e in intervals:
            if e < n_s:
                result.append([s, e])
            elif n_e < s:
                if not pushed:
                    result.append([n_s, n_e])
                result.append([s, e])
            else:
                n_s, n_e = [min(n_s, s), max(n_e, e)]
                
        if not pushed:
            result.append([n_s, n_e])
        
        return result
```
https://leetcode.com/problems/course-schedule-iii/ <br>
Sorting + max_heap <br />
Sort based on ending times, accept courses if possible <br />
If we can't check if we can remove something from the accepted set and select the one which makes our time compact