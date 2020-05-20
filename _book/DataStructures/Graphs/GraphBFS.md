### Graph BFS

Extremely powerful tool to have in the arsenal. <br />
Useful for a myriad of optimization problems. <br />
Quickest way to reach to X; even optimize cost function (Djikstra substitute using Priority Queue) <br />

https://leetcode.com/problems/keys-and-rooms/
```py
from collections import deque

class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        if not rooms:
            return True
        
        queue = deque()
        visited = set()
        
        queue.append(0)
        
        while queue:
            room = queue.popleft()
            visited.add(room)
            
            for v in rooms[room]:
                if v not in visited:
                    queue.append(v)
        
        return True if len(visited) == len(rooms) else False
```

Amazing priority queue method: <br />
https://leetcode.com/problems/network-delay-time/

```py
import heapq
from collections import deque, defaultdict

class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        graph = defaultdict(list)
        heap = []
        visited = {}
        
        if not N:
            return 0
        
        if K > N:
            return -1
        
        for u, v, w in times:
            graph[u].append((v, w))
           
        heap.append((0, K))
        max_time = 0
        
        while heap:
            time, node = heapq.heappop(heap)
            
            if node in visited:
                continue
                
            visited[node] = time
            max_time = max(max_time, time)
            
            for v, w in graph[node]:
                if v not in visited:
                    heapq.heappush(heap, (time + w, v))

        return max_time if len(visited) == N else -1
```
https://leetcode.com/problems/water-and-jug-problem
```py
from collections import deque

class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        queue = deque([(0, 0)])
        seen = set()
        
        if (x + y) < z:
            return False
        
        while queue:
            first, second = queue.popleft()
            if (first, second) in seen:
                continue 
            seen.add((first, second))
            
            if z in [first, second, first + second]:
                return True
            
            queue.append((first, y))
            queue.append((x, second))
            queue.append((0, second))
            queue.append((first, 0))
            
            total = (first + second)
            
            queue.append((min(total, x), total - min(total, x)))
            queue.append((total - min(total, y), min(total, y)))
                
        return False
```