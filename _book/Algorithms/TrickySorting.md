### Tricky Sorting

python `sorted` function can be fed a custom comparator. <br />
Coming up with a comparator is the most important part.

`functools.cmp_to_key(comparator)` can be used as a key to make this work.

https://leetcode.com/problems/largest-number/
```py
import functools

class Solution:
    def largestNumber(self, nums: List[int]) -> str:
        comparator = lambda x, y: 1 if x+y > y+x else -1 if x+y < y+x else 0
        s_nums = sorted([str(n) for n in nums], key=functools.cmp_to_key(comparator), reverse=True)
        largest = ''.join(s_nums)
        return str(int(largest))
```