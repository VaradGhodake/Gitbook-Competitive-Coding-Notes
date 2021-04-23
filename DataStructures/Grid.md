### Grid questions


Right-down problems are generally DP <br />
They follow overlapping substructures by default and optimal substructure trivially

#### DFS:
* https://leetcode.com/problems/path-with-maximum-gold/
* https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
* https://leetcode.com/problems/max-area-of-island/

Things that need to be taken care of:
1. directions
2. visited set and its implementation: sets or grid modification? Boundaries and visited check
3. Cache: depends on the problem e.g. checkout `longest increasing path in a matrix` question above
> sometimes cache can be used for visited tracking

Logic construction: <br />
Keep calm and think thoroughly. <br />
What's the sequence at each step? Do we start at an imaginary position before `(0, 0)` or on `(0, 0)`? <br />
We the solution can start on any of the indices in the grid (ie we are running 2 loops: on X and Y), we do following:
```sh
include current node -> dfs on children satisfying certain criteria -> return value to reflect the current node
```
DFS should directly return the answer.

#### BFS:
* Rotten oranges <br />
Tip: <br />
In such problems, implied visited through input modification is faster than visited set and step through queue element is faster than looping over queue size <br />
eg. https://leetcode.com/problems/shortest-path-in-binary-matrix/ <br />

https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
```py
from collections import deque

class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        queue = deque()
        k_left = k if grid[0][0] == 0 else k - 1
        X, Y = len(grid), len(grid[0])
        
        cache = {}
        queue.append((0, 0, k_left))
        
        steps = 0
        while queue:
            size = len(queue)
            
            for _ in range(size):
                x, y, k_left = queue.popleft()
                
                if k_left < 0:
                    continue
                
                if x == (X-1) and y == (Y-1):
                    return steps
                
                # cache for visited tracking
                prev_steps, prev_k = cache.get((x, y), (float('inf'), -1))
                if prev_steps <= steps and prev_k >= k_left:
                    continue
                cache[(x, y)] = (steps, k_left)
                
                # directions
                directions = [(x+1,y), (x-1,y), (x,y+1), (x,y-1)]
                for _x, _y in directions:
                    if not (0 <= _x < X) or not (0 <= _y < Y):
                        continue
                    
                    if k_left >= 0:
                        queue.append((_x, _y, k_left - grid[_x][_y]))
                
            steps += 1
        
        return -1
```
https://leetcode.com/problems/number-of-islands/
```py
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        islands = 0
        X, Y = len(grid), len(grid[0])
        
        def traverse(x, y):
            grid[x][y] = "0"
            
            directions = [(x+1,y), (x,y+1), (x-1,y), (x,y-1)]
            for x_dir, y_dir in directions:
                if not (0 <= x_dir < X) or not (0 <= y_dir < Y):
                    continue
                
                if grid[x_dir][y_dir] == "1":
                    traverse(x_dir, y_dir)
            
            return
        
        for _x in range(X):
            for _y in range(Y):
                if grid[_x][_y] == "1":
                    islands += 1
                    traverse(_x, _y)
        
        return islands
```