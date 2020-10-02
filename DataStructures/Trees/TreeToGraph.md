### Tree to graph

Sometimes, we need to convert a tree into a graph. <br />
Especially, if we want to traverse starting from an arbitary node.

Two approaches: <br />
* Memory efficient but altering the input data: <br />
Mark every node (add a new attribute to TreeNode object `parent`) <br />
Perform level-wise traversal from the target node where the neightbor loop will have `[node.parent, node.left, node.right]`

* Convert you tree into a graph: <br />
Will require additional O(n) space to store the whole thing <br />
Perform level-wise traversal starting from the target node

https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/ <br />
Note the graph construction step; keep in mind we have to filter out -1 from the iteration.
make sure there's a proper K value handling.
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import defaultdict, deque

class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, K: int) -> List[int]:
        if K == 0:
            return [target.val]
        
        graph = defaultdict(list)
        
        def traverse(node, parent):
            if not node:
                return
            
            graph[parent].append(node.val)
            graph[node.val].append(parent)
            traverse(node.left, node.val)
            traverse(node.right, node.val)
        
        traverse(root, -1)
        
        queue = deque()
        queue.append(target.val)
        visited = set()
        result = []
        K -= 1
        
        while queue:
            size = len(queue)
            
            for i in range(size):
                v = queue.popleft()
                visited.add(v)
                
                for nei in graph[v]:
                    if nei not in visited and nei != -1: 
                        if K != 0:
                            queue.append(nei)
                        
                        if K == 0:
                            result.append(nei)
            if K == 0:
                return result
            K -= 1
        
        return []
```