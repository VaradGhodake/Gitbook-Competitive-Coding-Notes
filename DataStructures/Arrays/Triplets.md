### Triplets

https://leetcode.com/problems/count-number-of-teams/
```py
class Solution:
    def numTeams(self, rating: List[int]) -> int:
        s = [0 for i in range(0, len(rating))]
        g = [0 for i in range(0, len(rating))]
        total = 0

        for i in range(1, len(rating)):
            for j in range(0, i):
                if rating[i] > rating[j]:
                    total += s[j]
                    s[i] += 1

                if rating[i] < rating[j]:
                    total += g[j]
                    g[i] += 1

        return total
```