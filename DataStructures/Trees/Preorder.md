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
https://leetcode.com/problems/add-one-row-to-tree/ <br />
Decent question, almost like a linked list question
```py
class Solution:
    def addOneRow(self, root: TreeNode, v: int, d: int) -> TreeNode:
        if d == 1:
            new = TreeNode(v)
            new.left = root
            return new
        
        def preorder(node, depth):
            if not node or depth == d:
                return None
            
            if depth == (d - 1):
                new = TreeNode(v)
                new.left = node.left
                node.left = new
                
                new = TreeNode(v)
                new.right = node.right
                node.right = new
            
            preorder(node.left, depth + 1)
            preorder(node.right, depth + 1)
            
            return node
        
        return preorder(root, 1)
```
https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/ <br />
Just be concerned about the state at the current node. Default start should be 1 since a single node satisfies the requirement <br />
Adjust based on the current node and then think about global max length 
```py
class Solution:
    def longestConsecutive(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        def preorder(node, prev, length):
            if not node:
                return
            
            if (node.val - prev) == 1:
                length += 1
            else:
                length = 1
            
            preorder.longest = max(preorder.longest, length)
            
            preorder(node.left, node.val, length)
            preorder(node.right, node.val, length)
            
            return
        
        preorder.longest = 0
        preorder(root, float('-inf'), 0)
        return preorder.longest
```
https://leetcode.com/problems/flatten-binary-tree-to-linked-list/ <br />
Check a similar one in our inorder set: `tree -> double linked list`
```py
class Solution:
    def flatten(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        if not root:
            return root
        
        def helper(node):
            if not node:
                return
            """
            We have to preseve the right pointer because the node's
            right pointer is going to be modified.
            
            Static variable to store the last node reference.
            """
            node_right = node.right
            
            if not helper.last:
                helper.first = node
            else:
                helper.last.right = node
                helper.last.left = None
                
            helper.last = node
            
            helper(node.left)
            helper(node_right)
        
        helper.first = helper.last = None
        helper(root)
        
        return helper.first
```