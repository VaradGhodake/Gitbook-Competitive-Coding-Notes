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
* https://leetcode.com/problems/jump-game-ii/
* Rotten oranges
https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
```py
from collections import deque

class Solution:
    def shortestPath(self, grid: List[List[int]], k: int) -> int:
        X = len(grid)
        Y = len(grid[0])
        
        visited = [[[-1, float('inf')] for i in range(0, Y)] for j in range(0, X)]
        # x, y, steps, k-left
        queue = deque([[0, 0, 0, k]])
        
        while queue:
            x, y, steps, k_left = queue.popleft()
            
            if x < 0 or x >= X or y < 0 or y >= Y:
                continue
            
            if grid[x][y]:
                k_left -= 1
            
            if k_left < 0:
                continue
            
            if visited[x][y][0] >= k_left and visited[x][y][1] <= steps:
                continue
            
            visited[x][y][0] = k_left
            visited[x][y][1] = steps
            
            if x == (X - 1) and y == (Y - 1):
                return steps
            
            queue.append([x - 1, y, steps + 1, k_left])
            queue.append([x + 1, y, steps + 1, k_left])
            queue.append([x, y - 1, steps + 1, k_left])
            queue.append([x, y + 1, steps + 1, k_left])
            
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