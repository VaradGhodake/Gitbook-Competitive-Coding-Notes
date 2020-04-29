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