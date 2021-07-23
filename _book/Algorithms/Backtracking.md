### Backtracking and Complete Search

Basic questions: <br />
Two choices:
* Don't choose the element: Just make a recursive call for the next element
* Choose the element and stay on the same if repeatition is allowed otherwise, go ahead. 
https://leetcode.com/problems/combination-sum/
```py
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        
        def backtrack(i, bucket, current):
            if current == target:
                result.append(bucket[:])
                return 
            
            if current > target or i == len(candidates):
                return
            
            backtrack(i + 1, bucket, current)
            backtrack(i, bucket + [candidates[i]], current + candidates[i])
            
        backtrack(0, [], 0)
        return result
```

#### Exhaustive search for an answer:
Check every possible option and see if a solution exists: (very similar to permutation problems) <br />
https://leetcode.com/problems/jump-game <br />
Accept one of the options at each step <br />
Emptying the bucket is not necessary 
```java
public class Solution {
    public boolean canJumpFromPosition(int position, int[] nums) {
        if (position == nums.length - 1) {
            return true;
        }

        int furthestJump = Math.min(position + nums[position], nums.length - 1);
        for (int nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++) {
            if (canJumpFromPosition(nextPosition, nums)) {
                return true;
            }
        }
        return false;
    }

    public boolean canJump(int[] nums) {
        return canJumpFromPosition(0, nums);
    }
}
```

#### Grid DFS

Solution of https://leetcode.com/problems/path-with-maximum-gold/ :
```py
def dfs(self, i: int, j: int, sum: int, seen: set) -> int:
    # constraints
    if(i < 0 or i >= m or j < 0 or j >= n or not grid[i][j] or (i, j) in seen):
        return sum
    # add to the 'visited' set
    seen.add((i, j))
    # update current target
    sum += grid[i][j]
    # set maximum target
    maximumSum = 0

    # directions to move in
    for x, y in ((i, j + 1), (i , j - 1), (i + 1, j), (i - 1, j)):
    # The actual recursion step
        maximumSum = max(self.dfs(x, y, sum, seen), maximumSum)
    # remove from the 'visited' set after recursion 
    seen.discard((i, j))
    return maximumSum

def getMaximumGold(self, grid: List[List[int]]) -> int:
    m, n = len(grid), len(grid[0])
    return max(self.dfs(i, j, 0, set()) for j in range(n) for i in range(m))
```

https://leetcode.com/problems/2-keys-keyboard/submissions/ <br />
Return types should be taken care of. <br  />

* Base condition
* Limiting condition if required (Use INTMAX for min recursion, INTMIN for max recursion)
* Actual recursion

```py
class Solution:
    def _helperCopyPaste(self, n: int, current: int, copied: int) -> int:
        if(current == n):
            return 0
        if(current > n):
            return 3000
        return min(2 + self._helperCopyPaste(n, current + current, current),
                   1 + self._helperCopyPaste(n, current + copied, copied))

    def minSteps(self, n: int) -> int:
        if(n == 1):
            return 0
        return 1 + self._helperCopyPaste(n, 1, 1)
```

https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/ <br />
For selected param, we can use bits. For memo array, we can use `functools.lru_cache` 

#### Generate parentheses
Keep a count of open and closed ones. <br />
Success cases: string length 2 * n and stack == 0 <br />
Failure cases: string length 2 * n and stack != 0 OR stack < 0 OR stack > n <br />
We just backtrack:
```py
            backtrack(current + '(', stack + 1)
            backtrack(current + ')', stack - 1)
```

### Combinatorics

#### Subsets
Reference: CC Cheat sheet book <br />
Note the number of recursive calls made

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
https://leetcode.com/problems/largest-time-for-given-digits/
```py
class Solution:
    def largestTimeFromDigits(self, A: List[int]) -> str:
        D = sorted(A, reverse=True)
        included = [False] * 4
        bucket = ""
        self.T = ""
        
        def permute(bucket):
            if self.T:
                return
            
            n = len(bucket)
            if n == 1 and int(bucket) > 2:
                return
            
            if n == 2 and int(bucket) > 23:
                return
            
            if n == 3 and int(bucket[2]) > 5:
                return
            
            if n == 4 and int(bucket[2: 4]) > 59:
                return 
            
            if n == 4:
                self.T = bucket[0: 2] + ':' + bucket[2: 4]
            
            for i in range(0, 4):
                if included[i]:
                    continue
                
                included[i] = True
                permute(bucket + str(D[i]))
                included[i] = False
                
        permute(bucket)
        return self.T
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
