### Graph coloring

Basically, bifurcation problems

* https://leetcode.com/problems/flower-planting-with-no-adjacent/ <br />
These set of problems are extremely similar with a code-changing difference <br />
The first one is solved with keeping in mind that the solution exists, we just have to find it <br />
For the these, we want to see if the solution exists: (use BFS)
* https://leetcode.com/problems/is-graph-bipartite/
* https://leetcode.com/problems/possible-bipartition/ <br />

_Tips:_ <br />
Make sure you have a proper representation of graph <br />
Run the first loop over all vertices so that you don't miss disconnected sections.

```py
from collections import deque

class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        visited = [0 for i in range(0, len(graph))]
        
        def bfs(i):
            queue = deque()
            queue.append((i, 1))
            
            while queue:
                node, color = queue.popleft()
                
                if visited[node] and visited[node] != color:
                    return False
                
                if visited[node]:
                    continue
                
                visited[node] = color
                
                colors = set([1, 2])
                colors.remove(color)
                color_left = colors.pop()
                
                for v in graph[node]:
                    queue.append((v, color_left))
            
            return True
        
        for s in range(0, len(graph)):
            if not visited[s]:
                if not bfs(s):
                    return False
                
        return True
```

Solution for the first one: 
Just filling up colors

```py
from collections import defaultdict

class Solution:
    def gardenNoAdj(self, N: int, paths: List[List[int]]) -> List[int]:
        graph = defaultdict(list)
        
        for start, end in paths:
            graph[start].append(end)
            graph[end].append(start)
            
        planted = [0 for i in range(0, N)]
        
        for i in range(1, N + 1):
            neigh = graph[i]
            if planted[i - 1]:
                continue
                
            flowers = set([1, 2, 3, 4])
            for n in neigh:
                if planted[n - 1] in flowers:
                    flowers.remove(planted[n - 1])
                
            planted[i - 1] = flowers.pop()
        
        return planted
```