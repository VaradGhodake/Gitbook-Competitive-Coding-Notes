### Topology Sort

<img src="../../static/tolologysort1.png" height=300 width=300 /> <br />
https://leetcode.com/problems/course-schedule-ii/
```py
from collections import deque, defaultdict

class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = defaultdict(set)
        incoming = defaultdict(set)
        queue = deque()
        u_set = set(range(0, numCourses))
        topo = []
        
        for c, p in prerequisites:
            graph[c].add(p)
            incoming[p].add(c)
            
            if p in u_set:
                u_set.remove(p)
        
        for node in u_set:
            queue.append(node)
            
        while queue:
            node = queue.popleft()
            topo.append(node)
            
            for nei in graph[node]:
                incoming[nei].remove(node)
                
                if not incoming[nei]:
                    queue.append(nei)
        
        return topo[::-1] if len(topo) == numCourses else []
```

https://leetcode.com/problems/course-schedule-iv/
```py
from collections import defaultdict, deque

class Solution:
    def checkIfPrerequisite(self, n: int, prerequisites: List[List[int]], queries: List[List[int]]) -> List[bool]:
        graph = defaultdict(list)
        incoming = defaultdict(set)
        pre = defaultdict(set)
        
        starts = set(range(0, n))
        queue = deque()
        
        for p, c in prerequisites:
            graph[c].append(p)
            incoming[p].add(c)
            
            if p in starts:
                starts.remove(p)
                
        for s in starts:
            queue.append(s)
            
        while queue:
            c = queue.popleft()
            
            for n in graph[c]:
                incoming[n].remove(c)
                pre[n] = pre[n].union(pre[c].union(set([c])))
                
                if not incoming[n]:
                    queue.append(n)
                    
        return [c in pre[p] for p, c in queries]
```