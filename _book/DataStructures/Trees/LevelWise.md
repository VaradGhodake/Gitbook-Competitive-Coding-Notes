### Levelwise traversals
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