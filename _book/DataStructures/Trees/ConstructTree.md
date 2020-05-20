### Tree construction questions

#### Balanced tree construction
https://leetcode.com/problems/balance-a-binary-search-tree/discuss/540038/python-3-easy-to-understand

```py
class Solution:
    def constructTree(arr: list) -> TreeNode:
        if not len(list):
            return None

        mid = len(arr) // 2
        
        node = TreeNode(arr[mid])
        node.left = self.constructTree(arr[:mid])
        node.right = self.constructTree(arr[mid + 1: ])

        return node
```

### Construction from preorder 
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