### Graph DFS

One path at a time type questions eg. Course Scheduling <br />
Visit the node -> process children -> unvisit it <br />
If there's a cycle, we visit the already locally visited node again <br />
If we have visited the node before globally, just exit with `True` as we have already checked and didn't find any cycle <br />
https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/ <br />
Global `visited` and local `dfs.visited` sets to not revisit nodes. <br />
Gotcha: make sure compare an incoming node to `visited` instead of `dfs.visited` to save time
```py
class Solution:
    def countComponents(self, n: int, edges: List[List[int]]) -> int:
        visited = set()
        connected = 0
        graph = {}
        
        
        for s, e in edges:
            graph[s] = graph.get(s, []) + [e]
            graph[e] = graph.get(e, []) + [s]
        
        def dfs(index):
            # gotcha
            if index in visited:
                return
            
            visited.add(index)
            dfs.visited.add(index)
            for n in graph.get(index, []):
                if n not in dfs.visited:
                    dfs(n)
            
            dfs.visited.remove(index)
            return
        
        dfs.visited = set()
        for i in range(n):
            if i not in visited:
                dfs(i)
                connected += 1
        
        return connected
```
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
https://leetcode.com/problems/parallel-courses/ <br />
The important thing to understand is that the deepest path will require the longest sem <br />
The question then boils down to finding this path. <br />
We return max depth including the current node
*Note:* Do not do normal DFS and depth tracking with a static variable. cache approach is efficient

Follow up: try to simplify cycle finding and returning method
```py
class Solution:
    def minimumSemesters(self, n: int, relations: List[List[int]]) -> int:
        self.valid = True
        visited = set()
        prereq = {}
        
        for p, c in relations:
            prereq[p] = prereq.get(p, []) + [c]
        
        def dfs(course):
            if not self.valid:
                return -1
                        
            if course in visited:
                self.valid = False
                return -1
            
            if course in cache:
                return cache[course]
            
            visited.add(course)
            
            max_next = 0
            for c in prereq.get(course, []):
                max_next = max(max_next, dfs(c))
            
            visited.remove(course)
            cache[course] = max_next + 1
            return cache[course]
        
        cache = {}
        max_depth = 0
        for course in range(1, n + 1):
            course_depth = dfs(course)
            
            if not self.valid:
                return -1
            
            max_depth = max(max_depth, course_depth)
        
        return max_depth
```
https://leetcode.com/problems/reconstruct-itinerary/ <br />
We need to store visited edges instead of visited nodes. <br />
Edges are stored as defaultdicts because the possibility of them repeating <br />
Edges are sorted because we need to return the result that comes first lexographically
```py
from collections import defaultdict

class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        graph = defaultdict(list)
        edges = defaultdict(int)
        
        self.itinerary = []
        
        for s, d in tickets:
            graph[s].append(d)
            edges[(s, d)] += 1
            
        for s in graph:
            graph[s] = sorted(graph[s])
        
        def traverse(current, path):
            if self.itinerary:
                return
            
            if len(path) == (len(tickets) + 1):
                self.itinerary = path[:]
            
            for n in graph[current]:
                if edges[(current, n)]:
                    edges[(current, n)] -= 1
                    traverse(n, path + [n])
                    edges[(current, n)] += 1
        
        traverse('JFK', ['JFK'])
        return self.itinerary
```
Great question: Looks like a tree question but it's not! <br />
Realized that the edges can be bidirectional. Just needed to add `visited` array and augment the graph init loop <br />
_Trees are just graphs minus the posibility of looping_ <br />
Thought process similar to that of tree DFS. Fetch consolidated data from the leaves and calculate the current one (postorder). <br />
https://leetcode.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/

```py
from collections import defaultdict

class Solution:
    def countSubTrees(self, n: int, edges: List[List[int]], labels: str) -> List[int]:
        result = [1] * n
        graph = defaultdict(list)
        visited = set()
        
        for s, d in edges:
            graph[s].append(d)
            graph[d].append(s)
     
        def traverse(node):
            if node in visited:
                return {}
            
            visited.add(node)
            
            if not graph[node]:
                return {labels[node]: 1}
            
            info = defaultdict(int)
            for v in graph[node]:
                for L, F in traverse(v).items():
                    info[L] += F
            
            info[labels[node]] += 1
            result[node] = info[labels[node]]
            return info
        
        traverse(0)
        return result
```