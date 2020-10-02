### Graph coloring

Basically, bifurcation problems

* https://leetcode.com/problems/flower-planting-with-no-adjacent/ <br />
These set of problems are extremely similar with a code-changing difference <br />
The first one is solved with keeping in mind that the solution exists, we just have to find it <br />
For the these, we want to see if the solution exists: (use BFS)
* https://leetcode.com/problems/is-graph-bipartite/
* https://leetcode.com/problems/shortest-path-with-alternating-colors/
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
Edge coloring <br />
The key is to store color-node pairing in the `seen` set -> Allows us to traverse only "meaningful" cycles
```py
from collections import defaultdict, deque

class Solution:
    def shortestAlternatingPaths(self, n: int, red_edges: List[List[int]], blue_edges: List[List[int]]) -> List[int]:
        graph = defaultdict(list)
        red = 1
        blue = 2
        queue = deque()
        seen = set()
        result = [-1 for i in range(0, n)]
 
        for s, d in red_edges:
            graph[s].append([d, red])

        for s, d in blue_edges:
            graph[s].append([d, blue])

        queue.append((0, red, 0))
        queue.append((0, blue, 0))
        while queue:
            node, color, length = queue.popleft()
            
            if (node, color) in seen:
                continue
            seen.add((node, color))

            if result[node] == -1:
                result[node] = length

            for v, c in graph[node]:
                colors = set([red, blue])
                colors.remove(color)
                expected = colors.pop()
                if (c == expected):
                    queue.append((v, expected, length + 1))

        return result
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