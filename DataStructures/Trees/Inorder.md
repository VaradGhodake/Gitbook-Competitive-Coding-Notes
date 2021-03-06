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
Another iterative: <br />
we use to exit the loop right after we find the discrepancy <br />
https://leetcode.com/problems/leaf-similar-trees/
```py
class Solution:
    def leafSimilar(self, root1: TreeNode, root2: TreeNode) -> bool:
        
        def iterative(root, validation=False):
            """
            validation true:= add elems to leaves attr
            validation false:= remove elems from the attr and exit if not match
            """
            current = root
            stack = []
            
            while stack or current:
                while current:
                    stack.append(current)
                    current = current.left
                
                current = stack.pop()
                
                if not current.left and not current.right:
                    if not validation:
                        iterative.leaves.append(current.val)
                    else:
                        if iterative.leaves and \
                        current.val == iterative.leaves[0]:
                            iterative.leaves.pop(0)
                        else:
                            iterative.similar = False
                            return
                
                current = current.right
        
        iterative.leaves = []
        iterative.similar = True
        iterative(root1)
        iterative(root2, validation=True)
        
        return iterative.similar and len(iterative.leaves) == 0
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
https://leetcode.com/problems/convert-bst-to-greater-tree/ <br />
Augmented inorder search; right first. DO NOT EVEN THINK ABOUT USING POSTORDER HERE. <br />
Use a static variable to keep track of the cumulative sum
```py
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        if not root or not (root.left or root.right):
            return root
        
        def _inorder(node):
            if not node:
                return None
            
            _inorder(node.right)
            
            node.val += _inorder.sum
            _inorder.sum = node.val
            
            _inorder(node.left)
            
            return node
        
        _inorder.sum = 0
        return _inorder(root)
```
https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/ <br />
Use a static variable to keep track of the last element. Returning the node won't work since node's right should be followed by node's parent.
```py
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        """
        The inorder bit is obvious.
        
        Now, the important thing to remember is that we are guaranteed 
        to get references to nodes left and right if we don't update 
        left before inorder(node.left) or before inorder(node.right)
        
        Setup step: 
        last.right = node
        node.left = last
        
        Rather than returning the node, just keep a static variable to keep
        track of the last visited node. Because, node's right should 
        be followed by the node's parent.
        
        If last node is not initialized, we need the first node.
        """
        
        if not root:
            return root
        
        def inorder(node):
            if not node:
                return
            
            inorder(node.left)
            
            if inorder.last:
                inorder.last.right = node
                node.left = inorder.last
            else:
                inorder.first = node
            
            # reference for the next element
            inorder.last = node
            
            inorder(node.right)
        
        inorder.first = inorder.last = None
        inorder(root)
        
        # make it cyclic
        inorder.last.right = inorder.first
        inorder.first.left = inorder.last
        
        return inorder.first
```