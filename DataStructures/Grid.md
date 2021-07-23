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

https://leetcode.com/problems/word-search
```py
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:      
        X, Y = len(board), len(board[0])
        
        def search(x, y, index):
            if index == (len(word)-1):
                return True
            
            search.visited.add((x, y))
            
            _dir = [(x+1, y), (x-1, y), (x, y+1), (x, y-1)]
            for _x, _y in _dir:
                if (0 <= _x < X) and (0 <= _y < Y) and ((_x,_y) not in search.visited):
                    if board[_x][_y] == word[index+1] and search(_x, _y, index+1):
                        return True
            
            search.visited.remove((x, y))
            return False
        
        search.visited = set()
        for row in range(X):
            for col in range(Y):
                if word[0] == board[row][col] and search(row, col, 0):
                    return True
        
        return False
```

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
# DFS solution
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

```py
# BFS solution
from collections import deque

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        X, Y = len(grid), len(grid[0])
        count = 0
        
        def traverse(_a, _b):
            queue = deque()
            
            queue.append((_a, _b))
            grid[_a][_b] = "0"
            
            while queue:
                x, y = queue.popleft()
                
                directions = [(x+1, y), (x-1, y), (x, y+1), (x, y-1)]
                
                for _x, _y in directions:
                    if (0 <= _x < X) and (0 <= _y < Y) and grid[_x][_y] == "1":
                        queue.append((_x, _y))
                        grid[_x][_y] = "0"
        
        for a in range(X):
            for b in range(Y):
                if grid[a][b] == "1":
                    traverse(a, b)
                    count += 1
        
        return count
```
https://leetcode.com/problems/shortest-bridge/ <br />
Paint first island negative and then perform BFS to find the first positive integer <br />\
As always, one check before pushing onto the queue and one check after popleft-ing
```py
from collections import deque

class Solution:
    def shortestBridge(self, A: List[List[int]]) -> int:
        X = len(A)
        Y = len(A[0])
        self.result = X + Y
        g_queue = deque()
        
        def paint_island(x, y):
            queue = deque()
            queue.append((x, y))
            
            while queue:
                x, y = queue.popleft()
                
                if A[x][y] < 0:
                    continue
                
                A[x][y] = -1
                g_queue.append((x, y, 0))
                
                directions = [(x-1,y), (x,y-1), (x+1,y), (x,y+1)]
                for _x, _y in directions:
                    if 0 <= _x < X and 0 <= _y < Y:
                        if A[_x][_y] > 0:
                            queue.append((_x, _y))
        
        
        def build_bridge():
            while g_queue:
                x, y, converted = g_queue.popleft()
                
                if A[x][y] == 2:
                    continue
                
                if A[x][y] == 0:
                    A[x][y] = 2
                
                directions = [(x-1,y), (x,y-1), (x+1,y), (x,y+1)]
                for _x, _y in directions:
                    if 0 <= _x < X and 0 <= _y < Y:
                        if A[_x][_y] == 1:
                            return converted
                        
                        if A[_x][_y] == 0:
                            g_queue.append((_x, _y, converted + 1))
        
        for x in range(X):
            for y in range(Y):
                if A[x][y] == 1:
                    paint_island(x, y)
                    result = build_bridge()
                    return result
```
https://leetcode.com/problems/shortest-path-to-get-food/
```py
from collections import deque

class Solution:
    def getFood(self, grid: List[List[str]]) -> int:
        X, Y = len(grid), len(grid[0])
        visited = set()
        
        queue = deque()
        for x in range(X):
            for y in range(Y):
                if grid[x][y] == '*':
                    queue.append((0, x, y))
                    visited.add((x, y))
                    break
        
        while queue:
            steps, x, y = queue.popleft()

            directions = [(x+1, y), (x, y+1), (x-1, y), (x, y-1)]
            
            for _x, _y in directions:
                if (0 <= _x < X) and (0 <= _y < Y) and (_x, _y) not in visited:
                    if grid[_x][_y] == '#':
                        return steps + 1
                    elif grid[_x][_y] == 'O':
                        visited.add((_x, _y))
                        queue.append((steps+1, _x, _y))
        
        return -1
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
#### Misc

https://leetcode.com/problems/spiral-matrix/
```py
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        """
        algorithm:
        Top side would be appended completely.
        Rest 3 will form a open bucket-like structure.
        "ownership" of the top-right: top row | bottom-right: right col
        top-left corner is obvious, bottom-left doesn't matter
        
        implementation:
        We maintain row_start, row_end, col_start, col_end.
        Update them after the whole traversal is done.
        After each run, make sure they are ready for the next one.
        Run it until row_start <= row_end and col_start <= col_end
        """
        # matrix metadata; init variables
        row_start, col_start = 0, 0
        row_end, col_end = len(matrix)-1, len(matrix[0])-1
        
        # result: to be returned
        result = []
        
        while row_start <= row_end and col_start <= col_end:
            # top
            for c in range(col_start, col_end + 1):
                result.append(matrix[row_start][c])
            
            # right
            for r in range(row_start+1, row_end+1):
                result.append(matrix[r][col_end])
            
            # bottom
            # This if condition is critical, avoid repetitons for single row matrices
            if row_end > row_start:
                for c in range(col_end-1, col_start, -1):
                    result.append(matrix[row_end][c])
            
            # left
            # This if condition is critical, avoid repetitons for single col matrices
            if col_end > col_start:
                for r in range(row_end, row_start, -1):
                    result.append(matrix[r][col_start])
            
            row_start += 1
            col_start += 1
            
            row_end -= 1
            col_end -= 1
        
        return result 
```
Rotate it layer by layer. Inspired from the solution of the previous one.
```py
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        
        n = len(matrix)
        
        # row-start, column-start
        r_s, c_s = 0, 0
        # row-end, column-end
        r_e, c_e = n-1, n-1
        
        # 1X1 square already rotated
        while n > 1:
            # stop before the other corner
            for i in range(n-1):
                targets = (
                            # bottom-left going upwards
                            matrix[r_e-i][c_s],
                            # top-left going rightwards
                            matrix[r_s][c_s+i],
                            # top-right going downwards
                            matrix[r_s+i][c_e],
                            # bottom-right going leftwards
                            matrix[r_e][c_e-i]
                          )
                matrix[r_s][c_s+i], matrix[r_s+i][c_e], matrix[r_e][c_e-i], matrix[r_e-i][c_s] = targets
                
            # move on to the inner square
            r_s += 1
            c_s += 1
            r_e -= 1
            c_e -= 1

            # reduce it by 2; not 1. Both r_s and c_e are contracting
            n -= 2
        
        return matrix
```
https://www.hackerrank.com/challenges/2d-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays
```py
def hourglassSum(arr):
    # Write your code here
    max_sum = float('-inf')
    X, Y = len(arr), len(arr[0])
    
    for x in range(X-2):
        for y in range(Y-2):
            _sum = arr[x][y] + arr[x][y+1] + arr[x][y+2] + \
                   arr[x+1][y+1] + \
                   arr[x+2][y] + arr[x+2][y+1] + arr[x+2][y+2]
            
            max_sum = max(max_sum, _sum)
        
    return max_sum
```