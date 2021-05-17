### Games

* https://leetcode.com/problems/nim-game/
* https://leetcode.com/problems/divisor-game/

https://leetcode.com/problems/nim-game/submissions/ <br />
There are winning positions and losing positions. We need to bring current position to a losing one so that the friend starts from the losing position i.e. numbers divisible by 4 in this case <br />
```py
class Solution:
    def canWinNim(self, n: int) -> bool:
        return not (n % 4 == 0)
```

https://leetcode.com/problems/divisor-game/ <br />
Same idea as above. We try to bring current situation into a losing one for the opponent. Use DP for that. Calculate one by one. Iterate over `0 < j < i` in the inner loop for search for divisibles and try to find at least one false value
