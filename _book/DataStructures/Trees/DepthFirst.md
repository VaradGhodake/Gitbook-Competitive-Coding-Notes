# Depth-First traversals

Inorder

```py
def inorder(root: TreeNode) -> None:
    if root:
        inorder(root.left)
        print(root.value)
        inorder(root.right)

```
###### Solutions are generally some varients of inorder code

#### Leaf-related problems: <br />
https://leetcode.com/problems/leaf-similar-trees/submissions/ 
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def leafSimilar(self, root1: TreeNode, root2: TreeNode) -> bool:
        leaves = []
        def seqBuilder(root):
            if root:
                if root.left == None and root.right == None:
                    leaves.append(root.val)
                seqBuilder(root.left)
                seqBuilder(root.right)
                
        seqBuilder(root1)
        leaves1 = leaves[0:]
        leaves = []
        seqBuilder(root2)

        if not (len(leaves) == len(leaves1)):
            return False
        
        for i in range(0, len(leaves)):
            if not (leaves[i] == leaves1[i]):
                return False
        
        return True      
```