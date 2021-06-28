# Graphs

Decent problemset: https://leetcode.com/discuss/general-discussion/655708/graph-problems-for-beginners-practice-problems-and-sample-solutions

* https://leetcode.com/problems/array-nesting/

* Course schedule IV

Good question: <br />
https://leetcode.com/problems/kill-process/ <br />
Points to note: 
Using graph vs tree (easier to construct and multiple children) <br />
Tagging/marking the target node: crucial step. Let everyone inherit parent tag if target found, 
tag it to be `True`, its subtree will automatically be the only set of nodes to have `True` tag <br />
Nodes that do not have children: children append step should have a `dict.get(node, [])` call 
```py
from collections import deque

class Solution:
    def killProcess(self, pid: List[int], ppid: List[int], kill: int) -> List[int]:
        graph = {}
        queue = deque()
        to_be_killed = []
        # 1
        for i, parent in enumerate(ppid):
            graph[parent] = graph.get(parent, []) + [pid[i]]
            
            if parent == 0:
                if pid[i] == kill:
                    return pid
                queue.append((pid[i], False))
        
        while queue:
            size = len(queue)
            
            for _ in range(size):
                p, tag = queue.popleft()
                
                if p == kill:
                    tag = True
                
                # 2
                if tag == True:
                    to_be_killed.append(p)
                
                #3
                for d in graph.get(p, []):
                    queue.append((d, tag))
        
        return to_be_killed
```