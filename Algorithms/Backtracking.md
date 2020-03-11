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