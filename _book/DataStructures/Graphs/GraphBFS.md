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
https://leetcode.com/problems/cheapest-flights-within-k-stops/ <br />
Two variables at play here. There's similar question for grid as well.
```py
import heapq
from collections import defaultdict

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        graph = defaultdict(list)
        visited = [float('inf') for i in range(0, n)]
        heap = []
        
        for u, v, w in flights:
            graph[u].append((v, w))
        
        heapq.heappush(heap, (0, src, 0))
        
        while heap:
            d, c, k = heapq.heappop(heap)
            
            if k >= visited[c] or k > (K + 1):
                continue
            
            if c == dst:
                return d
            
            visited[c] = d
            
            for n, n_d in graph[c]:
                heapq.heappush(heap, (d + n_d, n, k + 1))
            
        return -1
```