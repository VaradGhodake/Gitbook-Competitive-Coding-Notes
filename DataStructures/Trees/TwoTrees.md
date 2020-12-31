### Two trees

https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/ <br />
Python reference equality: `is` <br />
Compare it to the original one rather than the clone; the algorithm will take care of the result
```py
class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        self.result = None
        
        def inorder(o: TreeNode, c: TreeNode):
            if not o:
                return None
            
            inorder(o.left, c.left)
            if o is target:
                self.result = c
            inorder(o.right, c.right)
        
        inorder(original, cloned)
        return self.result
```

https://leetcode.com/problems/merge-two-binary-trees/
```py
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        
        def merge(node1, node2):
            if not node1 and not node2:
                return None
            
            if not node1:
                return node2
            
            if not node2:
                return node1
            
            node1.val += node2.val
            node1.left = merge(node1.left, node2.left)
            node1.right = merge(node1.right, node2.right)
            return node1
        
        return merge(t1, t2)
```
Keep things simple:<br />
https://leetcode.com/problems/all-elements-in-two-binary-search-trees/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from heapq import merge

class Solution:
    def getAllElements(self, root1: TreeNode, root2: TreeNode) -> List[int]:
        
        def inorder(node, l):
            if not node:
                return
            
            inorder(node.left, l)
            l.append(node.val)
            inorder(node.right, l)
        
        l1, l2 = [], []
        inorder(root1, l1)
        inorder(root2, l2)
        
        return merge(l1, l2)
```

An interesting use of generators: <br />
https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/discuss/537686/Python-Clean-and-Pythonic-way-using-iterator(generator)-solving-followup-too <br />
Explaination: https://leetcode.com/problems/find-a-corresponding-node-of-a-binary-tree-in-a-clone-of-that-tree/discuss/537686/Python-Clean-and-Pythonic-way-using-iterator(generator)-solving-followup-too/475462
```py
def it(node):
    if node:
        yield node
        yield from it(node.left)
        yield from it(node.right)
    
    # can directly iterate over this tree now
```