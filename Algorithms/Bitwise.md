### Bitwise 

Toggle j-th bit: `vowel_set ^= (1 << j)` <br />
We can use this to store all even or imperfect dict of a set (See longest substring vowel question)

https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/537/week-4-may-22nd-may-28th/3343/
```py
class Solution:
    def countBits(self, num: int) -> List[int]:
        bits = [0] * (num + 1)
        
        bits[0] = 0
        last_power = 0
        power_val = 1
        
        for i in range(1, num + 1):
            if i == power_val:
                last_power = power_val
                power_val *= 2
                bits[i] = 1
                continue
            
            if power_val > i:
                bits[i] = (bits[i - last_power] + 1)
                
        return bits
```
https://leetcode.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/
```py
class Solution:
    def findTheLongestSubstring(self, s: str) -> int:
        P = []
        imperfect = {64: -1}
        max_len = 0
        
        if not len(s):
            return 0
        
        vowels = set(['a', 'e', 'i', 'o', 'u'])
        vowel_set = 1 << 6
        
        for i, c in enumerate(s):
            if c in vowels:
                for j, v in enumerate(vowels):
                    if v == c:
                        vowel_set ^= (1 << j)
                        break

            if vowel_set in imperfect:
                max_len = max(max_len, i - imperfect[vowel_set])
            else:
                imperfect[vowel_set] = i

            P.append(vowel_set)
                
        return max_len
```
https://leetcode.com/problems/power-of-two/
Trick: If a number is a power of 2, `n & (n - 1) == 0`