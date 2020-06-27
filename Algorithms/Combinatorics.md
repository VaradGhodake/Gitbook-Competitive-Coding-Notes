### Combinatorics

#### Subsets
Reference: CC Cheat sheet book <br />
Note the number of recursive calls made

Two choices:
* Don't choose the element: Just make a recursive call 
* Choose the element
    1. Select step
    2. Go to the next depth
    3. Pop out so that we don't have to instantiate the bucket

```py
class SolutionGenerator:
    def solution_search(self, k : int, domain: list) -> None:
        if (k == self.n):
            self.powerset.append(self.subset[:])
            return
        self.solution_search(k + 1, domain)
        self.subset.append(domain[k])
        self.solution_search(k + 1, domain)
        self.subset.pop()

    def _helper_generator(self, domain: list) -> list:
        self.powerset = []
        self.subset = []
        self.n = len(domain)
        self.solution_search(0, domain)
        return self.powerset
```


#### Combinations: select r out of n (nCr)

* Recursion base case change (==r)
* Params of recursion: s: number of selected so far; k: array item index to be processed in that call
* Similar to normal combinations


```py
class T:
    def normalSubset(self, arr: list, r: int) -> list:
        nCr = []
        bucket = []
        n = len(arr)
        
        def helperNCR(s: int, k: int) -> None:
            if(s == r):
                nCr.append(bucket[:])
                return
            
            if(s > r or k == n):
                return
            
            helperNCR(s, k + 1)
            bucket.append(arr[k])
            helperNCR(s + 1, k + 1)
            bucket.pop()
        
        helperNCR(0, 0)
        return nCr```

#### Permutations

Recursive calls for every element in the loop <br />
No need to add a ‘reject’ scenario <br />
Accept, make changes and revert after going a level deep <br />
Filter right after the loop using a helper array used for tracking <br />
https://leetcode.com/problems/permutations/
```py
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return []
        
        result = []
        visited = [False for i in range(0, len(nums))]
        
        def backtrack(bucket):
            if len(bucket) == len(nums):
                result.append(bucket[:])
                return 
            
            for j in range(0, len(nums)):
                if visited[j]:
                    continue
                
                visited[j] = True
                backtrack(bucket + [nums[j]])
                visited[j] = False
                
        
        backtrack([])
        return result
```

Additional questions:
https://leetcode.com/problems/combination-sum/discuss/16510/Python-dfs-solution.

To allow repetition, go to the same node after select. <br />
```self.recurse(k, domain)```

#### Unique permutations
https://leetcode.com/problems/permutations-ii/
```py
from collections import Counter

class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        counter = Counter(nums)
        result = []
        
        def backtrack(bucket, counter):
            if len(bucket) == len(nums):
                result.append(bucket[:])
                return
            
            for num in counter:
                if not counter[num]:
                    continue
                    
                counter[num] -= 1
                backtrack(bucket + [num], counter)
                counter[num] += 1
        
        backtrack([], counter)
        return result
```