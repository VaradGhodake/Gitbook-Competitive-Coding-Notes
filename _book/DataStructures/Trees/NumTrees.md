### Number of Trees

https://leetcode.com/problems/binary-trees-with-factors/ <br />
Classic DP problem. Which means, find answers based on previous computations <br />
`number of tress that can be constructed from a node == amount from a smaller num * original num // possible factor` <br />
Sorting beforehand solves a lot of issues. We go in increasing order. DP takes care of 2 trees if nums are diffs or if the original num is a square
```py
class Solution:
    def numFactoredBinaryTrees(self, arr: List[int]) -> int:
        s_arr = sorted(arr)
        index = {num: i for i, num in enumerate(s_arr)}
        MOD = (10**9 + 7)
        
        trees = [1] * len(s_arr)
        total = len(s_arr)
        
        for i in range(1, len(s_arr)):
            for j in range(0, i):
                if s_arr[i] % s_arr[j] == 0:
                    left_index = j
                    right_index = index.get(s_arr[i] // s_arr[j], -1)
                    
                    if right_index == -1:
                        continue
                    
                    trees[i] += (trees[left_index] * trees[right_index])
                    total += (trees[left_index] * trees[right_index])
        
        return total % MOD
```