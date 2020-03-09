### Level-wise traversal problems

eg. Top-view, Bottom-view, Thickness of the tree, etc

* Can maintain the left-most node and right-most node or related data 
* Helper functions or helper data structures (especially collections.deque) for data transfer or retrieval (i.e. depth value or width value) can help

General solution template:

```py
from collections import deque, defaultdict

class Solution:
    def __init__(self):
        self.queue = deque()
        self.level_sum = defaultdict(int)
    
    def _helperMaxLevelSum(self) -> None:
        while(len(self.queue)):
            node, level = self.queue.popleft()
            if(not node):
                continue
            
            self.level_sum[level] += node.val
            
            self.queue.append((node.left, level + 1))
            self.queue.append((node.right, level + 1))
        
    def maxLevelSum(self, root: TreeNode) -> int:
        self.queue.append((root, 0))
        self._helperMaxLevelSum()
        
        return max(self.level_sum, key=self.level_sum.get) + 1
```