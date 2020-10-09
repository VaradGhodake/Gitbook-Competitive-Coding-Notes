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