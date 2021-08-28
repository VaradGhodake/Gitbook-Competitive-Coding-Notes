# DFS and BFS

##### Few notes:
* Use BFS if there's a need to go somewhere optimally. <br />
> eg. Minimum steps required to reach from A to B type questions, jump game questions
* Use DFS for exhaustive search in the grid where you have search everywhere, not optimal condition <br />
> eg. Find if you can create word X from the grid 
* Do not forget the `visited` grid

General solutions:
#### BFS
* collections.deque is extremely useful: use `append` and `popleft` for queue operations
* Run operations as long as the queue is not empty
* visited array is an important part of the solution to avoid repetition

Excellent questions:
* https://leetcode.com/problems/jump-game/ <br />
BFS is a little bit slower but gets the job done <br />
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
https://leetcode.com/problems/word-ladder/ <br />
short-hand fingerprints creation is a game changer here. Enables us to hop from one word to valid ones in O(1)
```py
from collections import defaultdict, deque

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        wordStore = defaultdict(list)
        
        for word in wordList:
            for i in range(len(word)):
                shortVersion = word[:i]+ '*' + word[i+1:]
                wordStore[shortVersion].append(word)
        
        queue = deque()
        
        queue.append((beginWord, 1))
        visited = set([beginWord])
        
        while queue:
            word, steps = queue.popleft()
            
            for i in range(len(word)):
                shortWords = wordStore[word[:i] + '*' + word[i+1:]]
                for next_word in shortWords:
                    if next_word not in visited:
                        if next_word == endWord:
                            return steps + 1
                        
                        visited.add(next_word)
                        queue.append((next_word, steps + 1))
        
        return 0
```
https://leetcode.com/problems/making-a-large-island/ <br />
REALLY GOOD! We traverse through 0 cells and try to connect as many islands in four directions as possible.
When we encounter an unvisited island, we color it with a static variable and increment the static variable for the next unencountered island. We make sure to cache the visited island's area based on its color.
```py
from collections import deque

class Solution:
    def largestIsland(self, grid: List[List[int]]) -> int:
        """
        - islands[color] = area
        We cache this info while marking
        
        - mark.id
        Color to mark the current island with
        """
        
        X, Y = len(grid), len(grid[0])
        # 0 cell is essentially an island with 
        # area 0
        islands = {0: 0}
        max_area = 0
        # edge case: all cells are 1
        zero_cell_exists = False
        
        def mark(_a, _b, color):
            queue = deque()
            queue.append((_a, _b))
            
            grid[_a][_b] = color
            # init area 1 because we temporarily mark the current
            # 0 cell as 1
            area = 1
            
            while queue:
                x, y = queue.popleft()
                
                directions = [(x+1,y), (x-1,y), (x,y+1), (x,y-1)]
                for _x, _y in directions:
                    if (0 <= _x < X) and (0 <= _y < Y):
                        if grid[_x][_y] == 1:
                            grid[_x][_y] = color
                            area += 1
                            
                            queue.append((_x, _y))
            # cache this!
            islands[color] = area
            return
        
        mark.id = 2
        for i in range(X):
            for j in range(Y):
                if grid[i][j] == 0:
                    zero_cell_exists = True
                    _area = 1
                    # if we encounter the same island again for single 0 cell
                    included_islands = set()
                    
                    # look at all the sorrounding cells and add their areas
                    directions = [(i+1,j), (i-1,j), (i,j+1), (i,j-1)]
                    
                    for _i, _j in directions:
                        if (0 <= _i < X) and (0 <= _j < Y):
                            if grid[_i][_j] in included_islands:
                                continue
                            
                            if grid[_i][_j] not in islands:
                                mark(_i, _j, mark.id)
                                
                                # init for the next marking function
                                mark.id += 1
                            
                            included_islands.add(grid[_i][_j])
                            _area += islands[grid[_i][_j]]
                            
                    max_area = max(max_area, _area)
        
        return max_area if zero_cell_exists else (X * Y)
```