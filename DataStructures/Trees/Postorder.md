### Postorder traversal

Sometimes we need to pass some values back up the tree. <br />
e.g. node's position from the bottom, max from right and left, check if the node exists in the left or right subtree. sum of the left subtree and right subtree <br />
Based on these properties, we need to find values of some other properties; use globals/ class attributes to record/update their value.

Optimization for BST: <br />
Check if the target value is greater than current, go right; no need to go left. Vice versa.

#### General solution:
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
```py
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        self.lca = None
        
        def traverse(node):
            if not node:
                return False
            
            L = traverse(node.left)
            R = traverse(node.right)
            
            if (L + R + (node.val in [p.val, q.val])) == 2:
                self.lca = node
                
            return L or R or (node.val in [p.val, q.val])
        
        traverse(root)
        return self.lca
```

https://leetcode.com/problems/most-frequent-subtree-sum/ <br />
_Note:_ Recording max_count while inserting so that finding max will be easier afterwards.

```py
from collections import deque, defaultdict

class Solution:
    def findFrequentTreeSum(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        
        sums = defaultdict(int)
        self.max_count = 0
        
        def traverse(node):
            if not node:
                return 0
            
            L = traverse(node.left)
            R = traverse(node.right)
            
            sums[L + R + node.val] += 1
            self.max_count = max(self.max_count, sums[L + R + node.val])
            
            return (L + R + node.val)
        
        traverse(root)
        result = []
        
        for key, value in sums.items():
            if self.max_count == value:
                result.append(key)
            
        return result
```
https://leetcode.com/problems/validate-binary-search-tree/
```py
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        self.valid = True
        
        if not root:
            return self.valid
        
        if not root.left and not root.right:
            return self.valid
        
        def recurse(node: TreeNode) -> int:
            if not node:
                return (float('-inf'), float('inf'))
            
            left_max, left_min = recurse(node.left)
            right_max, right_min = recurse(node.right)
            
            if (left_max >= node.val) or (right_min <= node.val):
                self.valid = False
                
            return (max(node.val, left_max, right_max), \
                    min(node.val, right_min, left_min))
        
        recurse(root)
        return self.valid
```
https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree/
```py
from collections import defaultdict

class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        tree = defaultdict(list)
        self.walk = 0
        visited = set()
        for s, e in edges:
            tree[s].append(e)
            tree[e].append(s)
            
        def traverse(node):
            visited.add(node)
            
            if not tree[node]:
                if hasApple[node]:
                    self.walk += 2
                return hasApple[node]
            
            apple_in_path = hasApple[node]
            for v in tree[node]:
                if v not in visited:
                    apple_in_path = traverse(v) or apple_in_path
            
            if apple_in_path and node:
                self.walk += 2
            
            return apple_in_path
            
        
        traverse(0)
        return self.walk
```