### Binary Search

Iterative solutions are quicker than the recursive ones. <br />
Recursive are cleaner though <br />

It's necessary to understand where BS terminates. This can answer a lot of questions.

Good problem sets:
* https://leetcode.com/discuss/general-discussion/691825/binary-search-for-beginners-problems-patterns-sample-solutions
* https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/discuss/686316/JavaC%2B%2BPython-Binary-Search

https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets
```py
from functools import lru_cache

class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        if len(bloomDay) < (m * k):
            return -1
        
        small = float('inf')
        large = float('-inf')
        
        for day in bloomDay:
            small = min(small, day)
            large = max(large, day)
        
        @lru_cache(maxsize=None)
        def verify(current):
            print(current)
            window = 0
            b_req = m
            
            for i, day in enumerate(bloomDay):
                if day <= current:
                    window += 1
                if day > current or i == (len(bloomDay) - 1):
                    b_req -= (window // k)

                    if b_req <= 0:
                        return True
                    
                    window = 0
            return False
        
        def binarySearch(start, end):
            if start > end:
                return -1
            
            mid = start + (end - start) // 2
            
            if verify(mid) and not verify(mid - 1):
                return mid
            
            if verify(mid) and verify(mid - 1):
                return binarySearch(start, mid - 1)
            
            return binarySearch(mid + 1, end)
        
        return binarySearch(small, large)
```