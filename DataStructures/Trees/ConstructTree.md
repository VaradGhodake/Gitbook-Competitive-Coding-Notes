### Tree construction questions


#### Construct a tree from the input
###### This is for leetcode style input
https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
```py
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        result = ''
        queue = deque()
        queue.append(root)
        
        while queue:
            size = len(queue)
            
            for _ in range(size):
                node = queue.popleft()
                
                if not node:
                    result += '#,'
                    continue
                
                result += (str(node.val) + ',')
                
                queue.append(node.left)
                queue.append(node.right)
        
        # ends with ','
        return result[:len(result)-1]

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        nodes = [TreeNode(int(val)) if val != '#' else None for val in data.split(',')]
        
        child, parent = 1, 0
        while child < len(nodes):
            if not nodes[parent]:
                parent += 1
                continue
                
            nodes[parent].left = nodes[child]
            child += 1
            nodes[parent].right = nodes[child]
            child += 1
            parent += 1
        
        return nodes[0]
```

#### Balanced tree construction
https://leetcode.com/problems/balance-a-binary-search-tree/
```py
class Solution:
    def balanceBST(self, root: TreeNode) -> TreeNode:
        node_vals = []
        
        def traverse(node):
            if not node:
                return 
            
            traverse(node.left)
            node_vals.append(node.val)
            traverse(node.right) 
        
        def construct_tree(start, end):
            if start > end:
                return None
            
            middle = start + (end - start) // 2

            current = TreeNode(node_vals[middle])
            current.left = construct_tree(start, middle - 1)
            current.right = construct_tree(middle + 1, end)
            
            return current            
        
        traverse(root)
        return construct_tree(0, len(node_vals) - 1)
```

#### Construction from preorder 
https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        
        if not preorder:
            return None
        
        def binarySearch(start, end, key) -> int:
            
            if start > end:
                return end + 1
            
            mid = start + (end - start) // 2
            
            if preorder[start] > key and preorder[start - 1] < key:
                return start
            
            if preorder[end] > key and preorder[end - 1] < key:
                return end        
            
            if preorder[mid] > key and preorder[mid - 1] < key:
                return mid
            
            if preorder[mid] < key:
                return binarySearch(mid + 1, end, key)
            
            if preorder[mid] > key:
                return binarySearch(start, mid - 1, key)
        
        def constructTree(start, end) -> TreeNode:
            if start > end:
                return None
            
            node = TreeNode(preorder[start])
            
            pivot = binarySearch(start + 1, end, preorder[start])
            node.left = constructTree(start + 1, pivot - 1)
            node.right = constructTree(pivot, end)
            
            return node
        
        return constructTree(0, len(preorder) - 1)
```

#### Some other constructions
https://leetcode.com/problems/maximum-binary-tree/
```py
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None
        
        def construct(l, r):
            if l >= r:
                return None
            
            max_index = l
            for i in range(l, r):
                if nums[i] > nums[max_index]:
                    max_index = i
            
            node = TreeNode()
            node.val = nums[max_index]
            
            node.left = construct(l, max_index)
            node.right = construct(max_index + 1, r)
            
            return node
        
        return construct(0, len(nums))
```
Important questions: <br />
https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/ <br />
Notice how we're using a static variable for construct function. <br />
Root is always the last unused element present in the postorder array. This element divides the inorder array in 2 parts. <br />
_Make sure you construct the right subtree before the left one!!_
```py
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        
        def construct(i_s, i_e):
            if i_s > i_e:
                return None
            
            root = TreeNode(postorder[construct.postorder_index])
            construct.postorder_index -= 1
            
            pivot = 0
            for i, e in enumerate(inorder):
                if e == root.val:
                    pivot = i
                    break
            
            root.right = construct(pivot + 1, i_e)
            root.left = construct(i_s, pivot - 1)
            
            return root
        
        construct.postorder_index = len(postorder) - 1
        return construct(0, len(inorder) - 1)
```