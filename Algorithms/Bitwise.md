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

https://leetcode.com/problems/add-binary/ <br />
Keep `a` smaller by check and swap. Write a rule function. <br />
One small check after the main loop.
```py
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        extra = 0
        result = ''
        
        def add(_a, _b, _c):
            one = 0
            
            for n in [_a, _b, _c]:
                if n == '1':
                    one += 1
            
            if one == 3:
                return '1', '1'
            if one == 2:
                return '1', '0'
            if one == 1:
                return '0', '1'
            if one == 0:
                return '0', '0'
        
        if len(a) > len(b):
            a, b = b, a
        
        i = 0
        while i < len(b):
            if i >= len(a):
                _a = '0'
            else:
                _a = a[len(a) - 1 - i]
            
            _b = b[len(b) - 1 - i]
            
            extra, num = add(_a, _b, extra)
            result = num + result
            
            i += 1
        
        if extra == '1':
            return '1' + result
        
        return result
```