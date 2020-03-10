### Backtracking and Complete Search

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


#### Permutations

Recursive calls for every element in the loop <br />
No need to add a ‘reject’ scenario <br />
Accept, make changes and revert after going a level deep <br />
Filter right after the loop using a helper array used for tracking <br />

```py
class Permutation:
    def perm_recur(self, k : int, domain: list) -> None:
        if(k == self.n):
            self.permutations.append(self.bucket[:])
            return
        for i in range(0, self.n):
            if(self.accepted[i]):
                continue
            self.accepted[i] = True
            self.bucket.append(domain[i])
            self.perm_recur(k + 1, domain)
            self.accepted[i] = False
            self.bucket.pop()

    def _helper(self, domain: list) -> list:
        self.n = len(domain)
        self.accepted = [False] * self.n
        self.permutations = []
        self.bucket = []
        self.perm_recur(0, domain)
        return self.permutations

```

Additional questions:
https://leetcode.com/problems/combination-sum/discuss/16510/Python-dfs-solution.

To allow repetition, go to the same node after select. <br />
```self.recurse(k, domain)```

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