### Heap questions:


#### Min/Max k based on some metric
If you want to find min k, use max_heap, otherwise min_heap since you want to compare the incoming element with the top of heap <br />
Only use the heap size of required number of elems: k <br />
If it overflows, pop. Be careful about the signs in case of max_heap <br />


https://leetcode.com/problems/course-schedule-iii/ <br />
Amazing question. We push if a new course from courses (sorted by end times) if able to be pushed. <br />
If not, our aim to make selections as compact as possible; so we displace the one with max duration (heap comes into picture here) and update `T`
```py
import heapq

class Solution:
    def scheduleCourse(self, courses: List[List[int]]) -> int:
        T = 0
        heap = []
        s_courses = sorted(courses, key=lambda x: (x[1], x[0]))
        
        for duration, last_day in s_courses:
            t = T + duration
            
            if t <= last_day:
                T = t
                heapq.heappush(heap, -duration)
            else:
                if heap and -heap[0] > duration:
                    _d = -heapq.heappop(heap)
                    heapq.heappush(heap, -duration)
                    T -= (_d - duration)
        
        return len(heap)
```
https://leetcode.com/problems/closest-binary-search-tree-value-ii/
if the `length of heap == k`, pop if the top of `max_heap` is greater than key
```py
import heapq

class Solution:
    def closestKValues(self, root: TreeNode, target: float, k: int) -> List[int]:
        heap = []
        
        def inorder(node):
            if not node:
                return
            
            inorder(node.left)
            
            if len(heap) == k and -heap[0][0] > abs(node.val - target):
                heapq.heappop(heap)

            if len(heap) < k:
                heapq.heappush(heap, (-abs(node.val - target), node.val))
                
            inorder(node.right)
        
        inorder(root)
        
        result = []
        while heap:
            _, val = heapq.heappop(heap)
            result.append(val)
        
        return result[::-1]
```
https://leetcode.com/problems/furthest-building-you-can-reach/ <br />
Great question! We allocate most diff b/w heights to ladders, obviously! (need heap to maintain that). <br />
We only substract bricks only if heap is overflowing.
```py
import heapq

class Solution:
    def furthestBuilding(self, heights: List[int], bricks: int, ladders: int) -> int:
        total = 0
        heap, TOP = [], 0
        bricks_left = bricks
        
        for i, h in enumerate(heights[1:], start=1):
            diff = (h - heights[i-1])
            if diff < 0:
                continue
            
            heapq.heappush(heap, diff)
            
            if len(heap) > ladders:
                bricks_left -= heapq.heappop(heap)
            
            if bricks_left < 0:
                return i-1
        
        return len(heights)-1
    
```
https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/ <br />
```py
import heapq

class Solution:
    def kWeakestRows(self, mat: List[List[int]], k: int) -> List[int]:
        heap = []
        result = []
        
        for index, row in enumerate(mat):
            soldiers = 0
            for p in row:
                if p == 0:
                    break
                soldiers += 1
            
            if len(heap) == k and soldiers >= -heap[0][0]:
                continue
            
            heapq.heappush(heap, (-soldiers, -index))
            
            if len(heap) == k + 1:
                heapq.heappop(heap)
        
        while heap:
            s, i = heapq.heappop(heap)
            result = [-i] + result
        
        return result
```
https://leetcode.com/problems/k-closest-points-to-origin/ <br />
Maintain a heap of size `K`
```py
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        heap, result = [], []
        DISTANCE = 0
        
        for x, y in points:
            current_distance = (x*x + y*y)
            
            if len(heap) == K and ((-1) * heap[0][DISTANCE]) < current_distance:
                continue
            
            heapq.heappush(heap, (-current_distance, (x, y)))
            
            if len(heap) == (K + 1):
                heapq.heappop(heap)
        
        while heap:
            _, point = heapq.heappop(heap)
            result.append(point)
        
        return result
```

#### Djikstra
https://leetcode.com/problems/path-with-minimum-effort/ <br />
Djikstra. Note where we're checking the existance in `visited` set. <br />
We'll always traverse min heuristic path because heap. (pay attention to the `heappush` call)
```py
import heapq

class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        X, Y = len(heights), len(heights[0])
        heap = [(0, 0, 0)] # d, x, y
        visited = set()
        
        while heap:
            d, x, y = heapq.heappop(heap)
            
            if (x, y) in visited:
                continue
            
            if x == (X-1) and y == (Y-1):
                return d
            
            visited.add((x, y))
            
            for _x, _y in [(x+1,y),(x-1,y),(x,y+1),(x,y-1)]:
                if not (0 <= _x < X) or not (0 <= _y < Y):
                    continue
                
                heapq.heappush(heap, (max(d, abs(heights[x][y]-heights[_x][_y])), _x, _y))
                
        return -1
```
https://leetcode.com/problems/merge-k-sorted-lists/
```py
import heapq

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        head = HEAD = None
        heap, iterator = [], len(lists) + 1
        
        for i, l in enumerate(lists):
            if l:
                heapq.heappush(heap, (l.val, i, l))
        
        while heap:
            min_val, _, l = heapq.heappop(heap)
            
            if HEAD is None:
                HEAD = ListNode(min_val)
                head = HEAD
            else:
                HEAD.next = ListNode(min_val)
                HEAD = HEAD.next
            
            if l.next:
                l = l.next
                heapq.heappush(heap, (l.val, iterator, l))
                iterator += 1
        
        return head
```
https://leetcode.com/problems/cheapest-flights-within-k-stops/
```py
import heapq

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        heap = []
        store = {}
        
        for s, e, d in flights:
            store[s] = store.get(s, []) + [(e, d)]
        
        heapq.heappush(heap, (0, 0, src))
        
        while heap:
            price, hops, city = heapq.heappop(heap)
            
            # we don't need `visited` array because of this constaint
            if hops > (K+1):
                continue
            
            if city == dst:
                return price
            
            for d, cost in store.get(city, []):
                heapq.heappush(heap, (price + cost, hops + 1, d))
        
        return -1
```
https://leetcode.com/problems/single-threaded-cpu/ <br />
Excellent question. Selecting min-heap is trivial but that `gotcha` here is that we need to sort them in an alternate array as well, injecting original indices into respective tasks. Based on `T`, push only that qualify into the heap. If the heap is empty and `T` is smaller than `task_idx` (the runner), prepare it for the next loop by assigning its starting value to `T`
```py
import heapq

class Solution:
    def getOrder(self, tasks: List[List[int]]) -> List[int]:
        tasks_sorted = sorted([[task[0], task[1], index] for index, task in enumerate(tasks)])
        task_idx = 0
        heap, result = [], []
        
        T = tasks_sorted[0][0]
        
        while len(result) < len(tasks):
            while task_idx < len(tasks) and T >= tasks_sorted[task_idx][0]:
                arrival, processing, index = tasks_sorted[task_idx]
                heapq.heappush(heap, (processing, index))
                task_idx += 1
            
            if heap:
                processing, index = heapq.heappop(heap)
                result.append(index)
                T += processing
            elif not heap and task_idx < len(tasks):
                T = tasks_sorted[task_idx][0]
        
        return result
        
```
https://leetcode.com/problems/find-k-closest-elements/
```py

```
