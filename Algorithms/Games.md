### Games

* https://leetcode.com/problems/nim-game/


https://leetcode.com/problems/nim-game/submissions/ <br />
There are winning positions and losing positions. We need to bring current position to a losing one so that the friend starts from the losing position i.e. numbers divisible by 4 in this case <br />
```py
class Solution:
    def canWinNim(self, n: int) -> bool:
        return not (n % 4 == 0)
```
