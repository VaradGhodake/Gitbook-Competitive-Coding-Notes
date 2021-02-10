### Left and right computations
Devide and conquer-esque

https://leetcode.com/problems/shortest-distance-to-target-color/ <br />
Good question. LO=Last Occurrence, DM=Distance Matrix <br />
Check earlier accepted solution. The answer is index distance from last occurence of that color on the left and then on the right. <br />
I've combined both of the loops here:
```py
class Solution:
    def shortestDistanceColor(self, colors: List[int], queries: List[List[int]]) -> List[int]:
        n = len(colors)
        
        LOL = {1:float('-inf'), 2:float('-inf'), 3:float('-inf')}
        LOR = {1:float('inf'), 2:float('inf'), 3:float('inf')}
        
        DM = [[float('inf'), float('inf'), float('inf')] for _ in range(len(colors))]
        
        for i in range(0, n):
            c_l = colors[i]
            c_r = colors[n-1-i]
            
            LOL[c_l] = i
            LOR[c_r] = n-1-i
            for col in [1, 2, 3]:
                DM[i][col-1] = min(DM[i][col-1], i-LOL[col])
                DM[n-1-i][col-1] = min(DM[n-1-i][col-1], LOR[col]-(n-1-i))
        
        return [DM[i][q-1] if DM[i][q-1] != float('inf') else -1 for i, q in queries]
```
https://leetcode.com/problems/shortest-distance-to-a-character/ <br />
Same thing; last observed left and last observed right
```py
class Solution:
    def shortestToChar(self, s: str, c: str) -> List[int]:
        DM = [float('inf')] * len(s)
        LOL, LOR = float('-inf'), float('inf')
        n = len(s)
        
        for i, sc in enumerate(s):
            if s[i] == c:
                LOL = i
            
            if s[n-1-i] == c:
                LOR = n-1-i
            
            DM[i] = min(DM[i], i-LOL)
            DM[n-1-i] = min(DM[n-1-i], LOR - (n-1-i))
        
        return DM
```