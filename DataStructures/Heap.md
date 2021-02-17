### Heap questions:


#### Min/Max k based on some metric
If you want to find min k, use max_heap, otherwise min_heap since you want to compare the incoming element with the top of heap <br />
Only use the heap size of required number of elems: k <br />
If it overflows, pop. Be careful about the signs in case of max_heap
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
https://leetcode.com/problems/merge-k-sorted-lists/submissions/
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