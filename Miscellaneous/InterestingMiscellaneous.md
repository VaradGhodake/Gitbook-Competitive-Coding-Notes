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

*31:* https://leetcode.com/problems/next-permutation/ <br />
Find the next just greater element, swap elements and reverse the remaining array <br />
Visualize the whole array and our case
```py
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        i = n - 2
        
        while i >= 0 and nums[i] >= nums[i+1]:
            i -= 1
        
        # non-increasing array
        if i == -1:
            nums.reverse()
            return 1
        
        j = i + 1
        while j < n and nums[j] > nums[i]:
            j += 1
        j -= 1
        
        nums[i], nums[j] = nums[j], nums[i]
        
        l = i + 1
        r = n - 1
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l += 1
            r -= 1
        
        return 0
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
https://leetcode.com/explore/challenge/card/january-leetcoding-challenge-2021/582/week-4-january-22nd-january-28th/3612/ <br />
Only 3 possiblities. If length diff > 1, not able to equilize using one change. <br />
Two loops for length diff == 1 and 0
```py
class Solution:
    def isOneEditDistance(self, s: str, t: str) -> bool:
        if abs(len(s)-len(t)) > 1:
            return False
        
        if abs(len(s)-len(t)) == 1:
            i = 0
            while i < min(len(s), len(t)):
                if s[i]==t[i]:
                    i += 1
                    continue
                
                return s[i:] == t[i+1:] if len(s) < len(t) else t[i:] == s[i+1:]            
            return True
        
        i = 0
        while i < len(s):
            if s[i] == t[i]:
                i += 1
                continue
            
            return s[i+1:] == t[i+1:]
        return False
```
https://leetcode.com/problems/squirrel-simulation/solution/ <br />
Great question. Distance finding is simple. Visualize how that'll happen. <br />
The first visit should be to the node closest to squirrel and farthest to tree <br />
`total - dist_t[m_idx] + dist_s[m_idx] == total - m` 
```py
class Solution:
    def minDistance(self, height: int, width: int, tree: List[int], squirrel: List[int], nuts: List[List[int]]) -> int:
        # distances from squirrel
        # distances from tree
        # max(t[x]-s[x])
        
        s, t = [[0] * len(nuts)] * 2
        m, m_idx = float('-inf'), 0
        total = 0
        
        for i, n in enumerate(nuts):
            x = n[0]
            y = n[1]
            
            dist_t = abs(tree[0] - x) + abs(tree[1] - y)
            dist_s = abs(squirrel[0] - x) + abs(squirrel[1] - y)
            
            total += 2 * dist_t
            dist_diff = (dist_t-dist_s)
            if m < dist_diff:
                m = dist_diff
                m_idx = i
        
        return total - m
```
#### Recursion
https://leetcode.com/problems/powx-n/ <br />
Calculate only half and based on power's divisibility by 2, take action. <br />
Stop when power is 0
```py
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            x = 1 / x
            n = -n
        
        def recurse(_n):
            if _n == 0:
                return 1
            
            half_pow = recurse(_n // 2)
            
            if _n % 2:
                return half_pow * half_pow * x
            else:
                return half_pow * half_pow
        
        return recurse(n)
```
https://leetcode.com/problems/broken-calculator/ <br />
Proof: https://leetcode.com/problems/broken-calculator/discuss/236565/Detailed-Proof-Of-Correctness-Greedy-Algorithm
```py
class Solution:
    def brokenCalc(self, X: int, Y: int) -> int:
        steps = 0
        
        while Y > X:
            if Y % 2:
                Y += 1
            else:
                Y //= 2
        
            steps += 1
        
        return steps + (X-Y)
```