### Backtracking and Complete Search

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