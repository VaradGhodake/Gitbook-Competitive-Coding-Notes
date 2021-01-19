### Levelwise traversals
-also useful for vertical order traversal

General solution: <br />
https://leetcode.com/problems/deepest-leaves-sum/
```py
from collections import deque

class Solution:
    def deepestLeavesSum(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        queue = deque()
        queue.append(root)
        current = root
        level_sum = 0
        
        while queue:
            size = len(queue)
            level_sum = 0
            
            while size:
                current = queue.popleft()
                level_sum += current.val
                
                if current.left:
                    queue.append(current.left)
                    
                if current.right:
                    queue.append(current.right)
                    
                size -= 1
        
        return level_sum
```
Append more values into the queue node <br />
https://leetcode.com/problems/cousins-in-binary-tree/
```py
from collections import deque


class Solution:
    def isCousins(self, root: TreeNode, x: int, y: int) -> bool:
        if not root:
            return False
        
        queue = deque()
        queue.append((root, -1))
        current = root
        
        
        while queue:
            size = len(queue)
            
            x_found = -1
            y_found = -1
            
            while size:
                
                current, parent = queue.popleft()
                
                if current.val == x:
                    x_found = parent
                
                if current.val == y:
                    y_found = parent
                
                if (x_found != -1) and (y_found != -1) and (x_found != y_found):
                    return True
                
                if (x_found == y_found) and (x_found != -1):
                    return False
                                     
                if current.left:
                    queue.append((current.left, current.val))
                    
                if current.right:
                    queue.append((current.right, current.val))
                
                size -= 1
                
        return False
```
https://leetcode.com/problems/binary-tree-vertical-order-traversal/ <br />
Level order traversal guarantees vertical sorting, just need to sort horizontally. <br />
Another brilliant idea is to store `min_x` and `max_x`. Then just return `[X[x] for x in range(min_x, max_x+1)]`.
This works because every `x` in that range will have atlease one node. (think about it)
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def verticalOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        
        X = {}
        queue = deque()
        queue.append((0, root))
        
        while queue:
            size = len(queue)
            
            for _ in range(size):
                x, node = queue.popleft()
                X[x] = X.get(x, []) + [node.val]
                
                if node.left:
                    queue.append((x-1, node.left))
                
                if node.right:
                    queue.append((x+1, node.right))
        
        return [X[x] for x in sorted(X)]
```