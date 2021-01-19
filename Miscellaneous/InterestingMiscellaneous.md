#### Need to find nth smallest or largest
Use max or min-heap or partial sort: quicksort variation <br />
A better way to achieve this is to create a heap of size `k` (target) first. For finding max k, use min-heap and vice-versa. <br />
https://leetcode.com/problems/kth-largest-element-in-an-array/

LRU cache:  <br /> https://leetcode.com/problems/lru-cache/ <br />
Simple dict py-3

#### Priority queues
*1383:* https://leetcode.com/contest/weekly-contest-180/problems/maximum-performance-of-a-team/ <br />
*Solution:* https://leetcode.com/problems/maximum-performance-of-a-team/discuss/539797/C%2B%2BPython-Priority-Queue

*857:* https://leetcode.com/problems/minimum-cost-to-hire-k-workers/

*295:* https://leetcode.com/problems/find-median-from-data-stream/ <br />
We maintain 2 heaps:- One max and one min <br />
Max heap contains smaller half nums and min one container bigger half. We keep them partially sorted this way. <br />
Push to the max heap first, and then perform rebalacing. Max heap should have 1 extra in case there are odd number of nums <br />

_remember:_ `heapq` gives us min heap by default, make it negative and push/pop from max heap
```py
import heapq

class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.max_heap = []
        self.min_heap = []

    def addNum(self, num: int) -> None:
        heapq.heappush(self.max_heap, -num)
        temp = heapq.heappop(self.max_heap)
        heapq.heappush(self.min_heap, -temp)

        if len(self.max_heap) < len(self.min_heap):
            temp = heapq.heappop(self.min_heap)
            heapq.heappush(self.max_heap, -temp)

    def findMedian(self) -> float:
        TOP = 0
        
        if len(self.max_heap) == len(self.min_heap):
            return ((-1) * self.max_heap[TOP] + self.min_heap[TOP]) / 2
        else:
            return (-1) * self.max_heap[TOP]
```

*621:* https://leetcode.com/problems/task-scheduler/ <br />
Make it compact. Consider all most frequency elems as a single character. <br />
Distance b/w them? of course `n` from the first one to make it as compact as possible. <br />
_IMP:_ If it's completely compact, we would never need any idle one. Just open up and absolve. <br />
Now just try to find the number of idle ones.
##### Count
```py
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        freq = {}
        max_freq, max_freq_count = 0, 0
        
        for i, task in enumerate(tasks):
            if task in freq:
                freq[task] += 1
            else:
                freq[task] = 1
            
            if max_freq == freq[task]:
                max_freq_count += 1
            elif max_freq < freq[task]:
                max_freq = freq[task]
                max_freq_count = 1
        
        if (max_freq_count - 1) >= n:
            # completely compact
            return len(tasks)
        
        slots_available = (n - (max_freq_count - 1)) * (max_freq - 1)
        tasks_remaining = len(tasks) - max_freq * max_freq_count
        
        if slots_available <= tasks_remaining:
            # no idles in this case
            return len(tasks)
        
        return len(tasks) + (slots_available - tasks_remaining) # tasks + idle slots
```

##### Finding the actual order:
A really good question. Used 3 different data structures. <br />
First of all, we need an always sorted list of tasks and their remaining frequency. <br />
`dict + max-heap` is the best combo here <br />
Then, the general algo for each cycle of steps (n + 1) is as follows: <br />
1. Pop from the heap and schedule it.
2. Push this to queue if it's remaining freq is > 1 (careful with the negative sign as it's a max-heap)
3. Increase the count by 1
4. If there's nothing in the heap AND the queue, return the result count

```py
import heapq

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        freq = dict()
        heap = []
        queue = []
        count = 0
        result = []
        
        for T in tasks:
            if freq.get(T, 0):
                freq[T] += 1
            else:
                freq[T] = 1
                
        for t, c in freq.items():
            heapq.heappush(heap, (-c, t))
        
        while heap:
            for i in range(0, n + 1):
                if heap:
                    c, t = heapq.heappop(heap)
                    result.append(t)
                    
                    if c != -1:    
                        queue.append((c + 1, t))
                    count += 1
                    
                    if not heap and not queue:
                        print(">> loop exit")
                        return count
                else:
                    count += 1
                    result.append('-')
            
            while queue:
                heapq.heappush(heap, queue.pop())
            
        return count
```

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
https://leetcode.com/problems/find-all-duplicates-in-an-array/ <br />
One of those questions where you'd have to read question description properly.
```py
class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        result = []
        
        for i, n in enumerate(nums):
            if nums[abs(n) - 1] < 0:
                result.append(abs(n))
                continue
            
            nums[abs(n) - 1] = -nums[abs(n) - 1]
        
        return result
```