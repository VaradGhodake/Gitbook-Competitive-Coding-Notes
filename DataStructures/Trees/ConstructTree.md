# Tree construction questions

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