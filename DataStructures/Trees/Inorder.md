### Inorder Traversal


It is useful when you just want to traverse the tree, if you find what you are looking for, set up a flag to make sure you don't recur unnecessary paths. Just return in the case of iterative <br />
Particularly useful in the case of BST as inorder will give you a sorted list
#### Iterative
https://leetcode.com/problems/sum-of-left-leaves/
```py
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        stack = []
        current = root
        total_sum = 0

        while stack or current:
            while current:
                stack.append(current)
                current = current.left
                if current and not current.left and not current.right:
                    total_sum += current.val

            current = stack.pop()
            current = current.right
            
        return total_sum
```
#### Recursive 
https://leetcode.com/problems/binary-tree-paths/ <br />
Return `False` only in the case of not satisfying the criteria, do not return `True` until the base case
```py
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        result = []
        
        def traverse(node, path):
            if not node:
                return
            
            if not node.left and not node.right:
                result.append(path + str(node.val))
                return            
            
            traverse(node.left, path + str(node.val) + "->")
            traverse(node.right, path + str(node.val) + "->")
            
        traverse(root, "")
        return result
```