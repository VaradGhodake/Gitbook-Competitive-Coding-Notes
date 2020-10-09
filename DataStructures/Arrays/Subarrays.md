### Subarrays

##### Optimization checks: <br />
1. Two pointers 
2. Sliding window with running values 
3. Pre and Post arrays <br />
There can be complex problems outside these as well

##### Find subarray size K:

Requires all subarrays of all sizes and find all where a constraint is matched: <br />
https://www.geeksforgeeks.org/number-subarrays-product-less-k/

Sliding window with two pointers <br />
Left move: if product is more than the constraint <br />
Right move: everytime unless the product is more than the constraint 

Catch: Each addition of the element produces `(end_index - (start_index - 1))` more subarrays <br />
Catch: Which implies -- subarray of size d produces `d * (d + 1)/2` different subarrays

https://www.geeksforgeeks.org/maximum-subarray-size-subarrays-size-sum-less-k/

Prefix array to store constraint related data (sorted) <br />
Binary search + two pointers to find ALL subarrays of that size satisfying the constraint

https://www.geeksforgeeks.org/number-of-subarrays-with-odd-sum/

Prefix array but instead of sum, store `sum modulo 2` <br />
`final_answer = prefix_arr[0] * prefix_arr[1]`


##### Operation on all of subarrays:
https://www.geeksforgeeks.org/xor-subarray-xors/

Catch: ith element frequency in all subarrays: `(i + 1) * (n - i)` <br />
Desired quantity at the end of the whole domain traversal 

https://leetcode.com/problems/maximum-subarray/

Keep current value running and max/min: <br />
* Including the element <br />
* Excluding the element: basically new subsequence start <br />

Looks like DP question but we don’t have to refer older values as we are dealing subarrays, not subsequences. Answer till  previous is stored in the comparison step of current and absolute max/min.

https://www.geeksforgeeks.org/maximum-product-subarray-set-3/

maxVal and minVal at each value during iteration. If negative, swap them before multi, other things similar to maximum subarray sum

Pre and post array
https://www.geeksforgeeks.org/maximum-length-of-strictly-increasing-sub-array-after-removing-at-most-one-element/

#### complex subarray problems
https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/

https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/discuss/87768/4-lines-Python

https://leetcode.com/problems/maximum-number-of-non-overlapping-subarrays-with-sum-equals-target/
```py
class Solution:
    def maxNonOverlapping(self, nums: List[int], target: int) -> int:
        prefix = [0]
        sum_pos = {0: 0}
        intervals = 0
        
        for i, n in enumerate(nums):
            prefix.append(prefix[-1] + n)
    
        for i, p in enumerate(prefix):
            if i == 0: continue
            
            if p - target in sum_pos:
                intervals += 1
                sum_pos = {}
            
            sum_pos[p] = i
        
        return intervals
```