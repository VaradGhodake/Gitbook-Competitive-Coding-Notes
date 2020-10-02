### Preorder traversal

Perfect for process node and pass value down the tree. <br />
Sometimes replacement for levelwise (NOT ALWAYS, OF COURSE) <br />
If we do not want to cover `not root.left` or `not root.right`, check if they exist and go down the tree. 

#### General solution:
https://leetcode.com/problems/sum-root-to-leaf-numbers/
```py
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        self.total = 0
        
        if not root:
            return 0
        
        def addNums(node, current):
            if not node.left and not node.right:
                self.total += (current * 10 + node.val)
                return
            
            if node.left:
                addNums(node.left, current * 10 + node.val)
            
            if node.right:
                addNums(node.right, current * 10 + node.val)
             
        addNums(root, 0)
        return self.total
```
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
```py
class Solution:
    def longestZigZag(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        self.max_length = 0
        left = -1
        right = 1
        
        def traverse(node, direction, length):
            if not node:
                return
            
            self.max_length  = max(self.max_length, length)
            
            if direction == left:
                traverse(node.left, left, 1)
                traverse(node.right, right, length + 1)
                
            elif direction == right:
                traverse(node.left, left, length + 1)
                traverse(node.right, right, 1)
                
            else:
                traverse(node.left, left, 1)
                traverse(node.right, right, 1)
            
        traverse(root, 0, 0)
        return self.max_length
```
https://leetcode.com/problems/path-sum-ii/
```py
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        result = []
        
        def traverse(node, path, current_sum):
            if not node:
                return
            
            if not node.left and not node.right:
                if current_sum == (sum - node.val):
                    path.append(node.val)
                    result.append(path)
                return
            
            traverse(node.left, path + [node.val], current_sum + node.val)
            traverse(node.right, path + [node.val], current_sum + node.val)

        traverse(root, [], 0)
        return result
```
https://leetcode.com/problems/path-sum-iii/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        self.result = 0
        
        def traverse(node, path_sum):
            if not node:
                return 
            
            if node.val == sum:
                self.result += 1
            
            new_path_sum = []
            for s in path_sum:
                new_sum = s + node.val
                
                if new_sum == sum:
                    self.result += 1
                    
                new_path_sum.append(new_sum)
            
            traverse(node.left, new_path_sum + [node.val])
            traverse(node.right, new_path_sum + [node.val])            
        
        traverse(root, [])
        return self.result
```