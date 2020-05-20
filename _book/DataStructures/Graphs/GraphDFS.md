### Graph DFS

One path at a time type questions eg. Course Scheduling <br />
Visit the node -> process children -> unvisit it <br />
If there's a cycle, we visit the already locally visited node again <br />
If we have visited the node before globally, just exit with `True` as we have already checked and didn't find any cycle <br />
https://leetcode.com/problems/course-schedule/
```py
from collections import defaultdict

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)
        g_visited = set()
        
        for c, p in prerequisites:
            graph[c].append(p)
        
        def dfs(i):
            if i in visited:
                return False
            
            if i in g_visited:
                return True
            
            if i not in g_visited:
                g_visited.add(i)
                
            visited.add(i)
            
            for j in graph[i]:
                if not dfs(j):
                    return False
                
            visited.remove(i)
            return True
            
        for i in range(0, numCourses):
            if i not in g_visited:
                visited = set()
                if not dfs(i):
                    return False
                
        return True
```

https://leetcode.com/problems/reconstruct-itinerary/ <br />
We need to store visited edges instead of visited nodes. <br />
Edges are stored as defaultdicts because the possibility of them repeating <br />
Edges are sorted because we need to return the result that comes first lexographically
```py
import heapq
from collections import defaultdict

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        if not tickets:
            return []
        
        graph = defaultdict(list)
        self.result = []
        edges = defaultdict(int)
        
        for d, a in tickets:
            graph[d].append(a)
            edges[d, a] += 1
        
        for s in graph:
            graph[s] = sorted(graph[s])
        
        edges[(None, "JFK")] += 1

        def dfs(start, end, result, selected):
            if edges[(start, end)] <= 0:
                return
            
            if selected == (len(tickets) + 1) and not self.result:
                self.result = result[:]
                return
            
            edges[(start, end)] -= 1

            for v in graph[end]:
                if not self.result:
                    dfs(end, v, result + [v], selected + 1)
                
            edges[(start, end)] += 1
        
        dfs(None, "JFK", ["JFK"], 1)
        return self.result
```