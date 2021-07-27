### Postorder traversal

Sometimes we need to pass some values back up the tree. <br />
e.g. node's position from the bottom, max from right and left, check if the node exists in the left or right subtree. sum of the left subtree and right subtree <br />
Based on these properties, we need to find values of some other properties; use globals/ class attributes to record/update their value.

Optimization for BST: <br />
Check if the target value is greater than current, go right; no need to go left. Vice versa. <br />
Also need to do the same sometimes to handle the nodes that don't have either left child or the right one. Don't forget to initialize L or R before calling postorder. Look at the solution of good leaf nodes.

Tree height questions:
* https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/
* https://leetcode.com/problems/find-leaves-of-binary-tree/

#### General solution:
https://leetcode.com/problems/binary-tree-maximum-path-sum/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        def postorder(node):
            if not node:
                # if every node has a negative val, we'd want to return 
                # the max negative val so returning `0` doesn't really work here
                return float('-inf')
            
            current = node.val
            L = postorder(node.left)
            R = postorder(node.right)
            
            # don't include if it's negative
            if L >= 0:
                current += L
            
            if R >= 0:
                current += R
            
            postorder.max_sum = max(postorder.max_sum, current)
            
            # we can return either left, right or the node itself
            # it won't be a path otherwise
            return max(
                        node.val,
                        node.val + L,
                        node.val + R
                   )
        
        # static variable to store max
        postorder.max_sum = float('-inf')
        postorder(root)
        
        return postorder.max_sum
```

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
https://leetcode.com/contest/weekly-contest-199/problems/number-of-good-leaf-nodes-pairs/ <br />
We send to send consolidated data up the tree; kinda of tricky and complex
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import defaultdict

class Solution:
    def countPairs(self, root: TreeNode, distance: int) -> int:
        self.total = 0
        
        def postorder(node):
            if not node.left and not node.right:
                return {1: 1}
            
            L, R = {0 : 0}, {0: 0}
            if node.left:
                L = postorder(node.left)
            if node.right:
                R = postorder(node.right)
        
            for d1, lc in L.items():
                for d2, rc in R.items():
                    if d1 + d2 <= distance and d1 and d2:
                        self.total += (lc * rc)
            
            leaves = defaultdict(int)
            for d, c in L.items():
                if d <= distance:
                    leaves[d + 1] += c
            
            for d, c in R.items():
                if d <= distance:
                    leaves[d + 1] += c
            
            return leaves
        
        postorder(root)
        return self.total
```
https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/ <br />
We even treat `None` nodes as leaves. Other leave nodes would obviously lower than the `None` node. <br />
If the left and right leaves are at the same height, just return the current node as LCA otherwise just pass it on with the adjusted height.
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def lcaDeepestLeaves(self, root: TreeNode) -> TreeNode:
        def postorder(node):
            if not node:
                return 0, None
            
            if not node.left and not node.right:
                return 1, node
            
            L_H, L_lca = postorder(node.left)
            R_H, R_lca = postorder(node.right)
            
            if L_H > R_H:
                return L_H + 1, L_lca
            
            if R_H > L_H:
                return R_H + 1, R_lca
            
            return L_H + 1, node
        
        _, lca = postorder(root)
        return lca
```
A slightly similar problem: <br />
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', nodes: 'List[TreeNode]') -> 'TreeNode':
        target = set([i.val for i in nodes])
        self.lca = None
        
        def postorder(node):
            if not node:
                return 0
            
            L = postorder(node.left)
            R = postorder(node.right)
            
            found = L + R + (node.val in target)
            if found == len(target) and not self.lca:
                self.lca = node
            
            return found
        
        postorder(root)
        return self.lca
```
https://leetcode.com/problems/find-leaves-of-binary-tree/ <br />
Interesting question! It indirectly asks us to group nodes with the same height together. Then, return them in increasing order of the height in question. <br />
Postorder guaratees that we process them from ground up, so the second requirement is implemented by default. Rest is just height calculation.
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findLeaves(self, root: TreeNode) -> List[List[int]]:
        store = {}
        
        def postorder(node):
            if not node:
                return -1
            
            # steps 1 and 2 can be merged in one-line

            # 1. find children's heights
            L = postorder(node.left)
            R = postorder(node.right)
            
            # 2. node's height
            H = 1 + max(L, R)

            # update the store; we can also use defaultdict(list) here
            store[H] = store.get(H, []) + [node.val]
            
            # return the height back to its parents
            return H
        
        # trigger
        postorder(root)

        # these will be sorted by default
        return store.values()
```
https://leetcode.com/problems/binary-tree-pruning/
```py
class Solution:
    def pruneTree(self, root: TreeNode) -> TreeNode:
        
        def postorder(node):
            if not node:
                return False
            
            L = postorder(node.left)
            R = postorder(node.right)
            
            if not L:
                node.left = None
            
            if not R:
                node.right = None
            
            return L or R or (node.val == 1)
        
        return root if postorder(root) else None
    
```
