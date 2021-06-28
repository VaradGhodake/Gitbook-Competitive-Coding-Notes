### Interval problems

Template for simple questions like: <br />
* 435 Non-overlapping Intervals 
* 56 Merge Intervals 
* 252 Meeting Rooms
* 253 Meeting Rooms II 
* https://leetcode.com/problems/interval-list-intersections/
* https://leetcode.com/problems/meeting-scheduler/
* https://leetcode.com/problems/partition-labels/ (secretly an interval problem; intervals are sorted automatically once you create because of the way we do them)

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
https://leetcode.com/problems/meeting-rooms-ii/ <br />
We just need events actually. If one comes, need a room if anyone leaves, required rooms decrease. <br />
Convert into a flat list, closing times should be changed into negative. IMP: for sorting to get the events, sort based on absolute vals and tiebreaker should be a sign. <br />
Negatives should be given a preference.
```py
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        n = len(intervals)
        
        intervals = [((-1)**i) * interval[i] for interval in intervals for i in range(2)]
        intervals = sorted(intervals, key=lambda x: (abs(x), x))
        
        rooms, max_rooms = 0, 1
        
        for event in intervals:
            if event >= 0:
                rooms += 1
            else:
                rooms -= 1
            
            max_rooms = max(max_rooms, rooms)
        
        return max_rooms
```s
Another variation of this problem: <br />
https://leetcode.com/problems/remove-covered-intervals/
```py
class Solution:
    def removeCoveredIntervals(self, intervals: List[List[int]]) -> int:
        # sort based on the start time and prioritize the longer interval
        intervals = sorted(intervals, key = lambda x: (x[0], -x[1]))
        current = intervals[0]
        covered = 0
        
        for s, e in intervals[1:]:
                if current[0] <= s and current[1] >= e:
                    covered += 1
                else:
                    current = [s, e]
        
        return len(intervals) - covered
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
https://leetcode.com/problems/interval-list-intersections/
```py
class Solution:
    def intervalIntersection(self, A, B):
        i, j, START, END = 0, 0, 0, 1
        max_start, min_end = float('-inf'), float('inf')
        result = []
        
        while i < len(A) and j < len(B):
            max_start = max(A[i][START], B[j][START])
            min_end = min(A[i][END], B[j][END])
            
            if max_start <= min_end:
                result.append([max_start, min_end])
            
            if A[i][END] < B[j][END]:
                i += 1
            else:
                j += 1
            
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
                    # can be optimized here 
                    # we can return 
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

https://leetcode.com/problems/partition-labels/
```py
class Solution:
    def partitionLabels(self, S: str) -> List[int]:
        # find intervals
        # merge intersecting intervals
        intervals = {}
        LAST_INTERVAL, END = -1, 1
        result = []
        
        for i, s in enumerate(S):
            if s in intervals:
                intervals[s][END] = i
            else:
                intervals[s] = [i, i]
        
        intervals = intervals.values()
        
        for s, e in intervals:
            if not result or result[LAST_INTERVAL][END] < s:
                result.append([s, e])
            else:
                result[LAST_INTERVAL][END] = max(result[LAST_INTERVAL][END], e)
        
        return [e-s+1 for s, e in result]
```
https://leetcode.com/problems/missing-ranges/ <br />
Interesting question. Iterate over array and compare to the prev elem, `[prev + 1:current - 1]` is the missing range. Be careful about the edge case of empty input
```py
class Solution:
    def findMissingRanges(self, nums: List[int], lower: int, upper: int) -> List[str]:
        if not nums:
            return [str(lower)] if lower == upper else [str(lower) + "->" + str(upper)]
        
        
        def addToResult(start, end):
            if start == end:
                result.append(str(start))
            elif start < end:
                result.append(str(start) + '->' + str(end))
        
        result = []
        for i, n in enumerate(nums):
            if i == 0:
                start = lower
                end = nums[0] - 1
                addToResult(start, end)
                continue
            
            start = nums[i-1] + 1
            end = n - 1
            
            addToResult(start, end)
        
        start = nums[-1] + 1
        end = upper
        
        addToResult(start, end)
        
        return result
```
https://leetcode.com/problems/meeting-scheduler/ <br />
Just find the first intersection with enough duration width <br />
Heap solution is also nice. Check LeetCode solutions
```py
class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:
        i = j = 0
        slots1 = sorted(slots1)
        slots2 = sorted(slots2)
        
        while i < len(slots1) and j < len(slots2):
            max_start = max(slots1[i][0], slots2[j][0])
            min_start = min(slots1[i][0], slots2[j][0])
            min_end   = min(slots1[i][1], slots2[j][1])
            max_end   = max(slots1[i][1], slots2[j][1])
            
            if (min_end - max_start) >= duration:
                    return [max_start, max_start + duration]
            
            if min_end == slots1[i][1]:
                i += 1
            else:
                j += 1
        
        return []
```
