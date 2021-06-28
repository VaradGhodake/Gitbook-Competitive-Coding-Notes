# DFS and BFS

##### Few notes:
* Use BFS if there's a need to go somewhere optimally. <br />
> eg. Minimum steps required to reach from A to B type questions, jump game questions
* Use DFS for exhaustive search in the grid where you have search everywhere, not optimal condition <br />
> eg. Find if you can create word X from the grid 
* Do not forget the `visited` grid

General solutions:
#### BFS
* collections.deque is extremely useful: use `apend` and `popleft` for queue operations
* Run operations as long as the queue is not empty
* visited array is an important part of the solution to avoid repetition

Excellent questions:
* https://leetcode.com/problems/jump-game/ <br />
DFS is a little bit slower but gets the job done <br />
Being greedy from the end to start works well 
* https://leetcode.com/problems/jump-game-ii/ <br />
* Water and jug problem
https://leetcode.com/problems/water-and-jug-problem
```py
from collections import deque

class Solution:
    def canMeasureWater(self, x: int, y: int, z: int) -> bool:
        queue = deque([(0, 0)])
        seen = set()
        
        if (x + y) < z:
            return False
        
        while queue:
            first, second = queue.popleft()
            if (first, second) in seen:
                continue 
            seen.add((first, second))
            
            if z in [first, second, first + second]:
                return True
            
            queue.append((first, y))
            queue.append((x, second))
            queue.append((0, second))
            queue.append((first, 0))
            
            total = (first + second)
            
            queue.append((min(total, x), total - min(total, x)))
            queue.append((total - min(total, y), min(total, y)))
                
        return False
```
https://leetcode.com/problems/open-the-lock/ <br />
Runtime: `O(N^2 * A^N + D)` <br />
1. N = length of lock combination, and substring operations on that string = N * N
2. A = value each wheen can take. Here, A == 10. There are A * N combinations possible
3. `deadends_set` creation

```py
from collections import deque

class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        deadends_set = set(deadends)
        INIT_STATE, moves = '0000', 0
        visited = set()
        
        if INIT_STATE in deadends_set:
            return -1
        elif INIT_STATE == target:
            return 0
        
        queue = deque()
        queue.append((INIT_STATE, 0))
        
        while queue:
            state, moves = queue.popleft()
            
            for i in range(0, len(state)):
                for direction in [1, -1]:
                    wheel = int(state[i])
                    next_state = state[:i] + str((wheel + direction) % 10) + state[i+1:]
                    
                    if next_state == target:
                        return moves + 1
                    
                    if next_state not in deadends_set and next_state not in visited:
                        visited.add(next_state)
                        queue.append((next_state, moves+1))
        
        return -1
```
#### DFS

* Do not forget the visited array (you'll end up with stack limit exceeded error otherwise)

https://leetcode.com/problems/word-search
```py
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        X = len(board)
        Y = len(board[0])

        visited = [[False for y in range(0, Y))] for x in range(X)]
        
        def dfs(letter, x, y) -> bool:
        # base case should be first
            if letter == len(word):
                return True
            
            if x >= X or x < 0 or y >= Y or y < 0:
                return False
            
            if visited[x][y]:
                return False
            
            if word[letter] == board[x][y]:
            # make sure you don't visit the already visited cell
                visited[x][y] = True

                if(
                    dfs(letter + 1, x + 1, y) or
                    dfs(letter + 1, x - 1, y) or
                    dfs(letter + 1, x, y + 1) or
                    dfs(letter + 1, x, y - 1)
                ):
                    return True

                visited[x][y] = False

            return False
              
        for i in range(0, len(board)):
            for j in range(0, len(board[i])):
                if word[0] == board[i][j]:
                    if dfs(0, i, j):
                        return True
                else:
                    continue
        
        return False
```
https://leetcode.com/problems/pacific-atlantic-water-flow/ <br />
Nice question, we traverse reverse; from the ends to BFS inwards. `>=` to proceed. <br />
We send init queues to the traverse function: this can get messy if not careful
```py
from collections import deque

class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        X = len(heights)
        Y = len(heights[0])
        
        def traverse(queue):
            visited = set()
            
            while queue:
                x, y = queue.popleft()
                
                if (x, y) in visited:
                    continue
                
                visited.add((x, y))
                
                _dir = [(x+1, y), (x-1, y), (x, y+1), (x, y-1)]
                for _x, _y in _dir:
                    if 0 <= _x < X and 0 <= _y < Y:
                        if heights[_x][_y] >= heights[x][y]:
                            if (_x, _y) not in visited:
                                queue.append((_x, _y))
            
            return visited
        
        pacific_queue = deque()
        atlantic_queue = deque()
        
        for x in range(X):
            pacific_queue.append((x, 0))
            atlantic_queue.append((x, Y-1))
        
        for y in range(Y):
            pacific_queue.append((0, y))
            atlantic_queue.append((X-1, y))
        
        pacific = traverse(pacific_queue)
        atlantic = traverse(atlantic_queue)
        
        return [[x, y] for x, y in pacific if (x, y) in atlantic]
```