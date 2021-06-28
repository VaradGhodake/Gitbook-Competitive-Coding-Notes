### Knapsack

###### Must do problems:
* https://leetcode.com/problems/ones-and-zeroes/
* https://leetcode.com/problems/coin-change/
* https://leetcode.com/problems/word-break/

#### When?

Generally we have to include/exclude an element and go ahead <br />
If we can repeat an element, we'd have to accept it a certain number of times in the answer and move ahead

We've been given a list of things with some attributes (x, y). Select max/min things we can select so that `sum(xi for thing in things) < X` and `sum(yi for thing in things) < Y` <br />

Sometimes, it's disguised under fitting things in a set (word-break). In that case, attributes of that target set are params to optimize against. Sometimes, it's true or false that one has to compute from previous results (word-break again)

##### Recursion


##### Memoization


##### DP
###### 1. Check if we can repeat an element or not; that determines a lot of things, espectially the DP loop
DP array should be `(X+1) * (Y+1)` (one place for none selected i.e. `[0][0]`) with `Xi` and `Yi` representing the case if the optimization criteria is `Xi * Yi` <br />
Example: `dp = [[0 for _ in range(Y+1)] for _ in range(X+1)]` <br />
Be smart: try to simplify the checks inside main loops for (if it can fit here: eg. word-break) <br />
If we shoudn't repeat, first loop should be the one of items, second should be over DP array. Otherwise, reverse order; Think about it, makes perfect sense.
https://leetcode.com/problems/ones-and-zeroes/
```py        
        for string in strs:
            for x in range(X, -1, -1):
                for y in range(Y, -1, -1):
```
https://leetcode.com/problems/coin-change/
```py
        for i in range(1, amount + 1):
            for coin in coins:
```
https://leetcode.com/problems/word-break/
```py
        for x in range(X+1):
            for y in range(x):
```
###### 2. Table filling: Top-down and bottom-up

Now, this is interesting. We do not want overcounting. <br />
In a non-repetition case, prefer bottom-up because we don't want to include the same element again. <br />

We always compute the result for `[x][y]` based on `_x = [x-(xi)]` and `_y = [y-(yi)]` <br />
_Note:_ During traversal, make sure `_x` and `_y` are within the boundaries of the DP table <br />

Bottom-up: https://leetcode.com/problems/ones-and-zeroes/
```py
        for string in strs:
            for x in range(X, -1, -1):
                for y in range(Y, -1, -1):
                    
                    _x = x - freq[string]['0']
                    _y = y - freq[string]['1']
                    
                    if _x >= 0 and _y >= 0:
                        dp[x][y] = max(1 + dp[_x][_y], dp[x][y])
```
Top-down: https://leetcode.com/problems/coin-change/ 
```py
        for i in range(1, amount + 1):
            for coin in coins:
                if i < coin:
                    continue
                    
                dp[i] = min(dp[i], dp[i - coin] + 1)
```
