### Subarrays

##### Optimization checks: <br />
1. Two pointers 
2. Sliding window with running values 
3. Pre and Post arrays <br />
There can be complex problems outside these as well

##### Find subarray size K:
https://leetcode.com/problems/subarray-sum-equals-k/ <br />
`k = current - prev`; so `prev = current - k` <br />
Add all frequencies of prev to the total
```py
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        cont_sum, subarrays = 0, 0
        P = {cont_sum: 1}
        
        for num in nums:
            # calculate the prefix sum
            cont_sum += num
            
            # cont_sum - k = P[_x]
            subarrays += (P.get(cont_sum - k, 0))
            
            # update the sum store
            P[cont_sum] = P.get(cont_sum, 0) + 1
        
        return subarrays
```

Requires all subarrays of all sizes and find all where a constraint is matched: <br />
https://www.geeksforgeeks.org/number-subarrays-product-less-k/

Sliding window with two pointers <br />
Left move: if product is more than the constraint <br />
Right move: everytime unless the product is more than the constraint 

Catch: Each addition of the element produces `(end_index - (start_index - 1))` more subarrays <br />
Catch: Which implies -- subarray of size d produces `d * (d + 1)/2` different subarrays


https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/ <br />
Great question, instead of looking at the ends, find the max subarray with sum `total - x` <br />
Corner case would be when `total == x`. Use prefix array to find that
```py
class Solution:
    def minOperations(self, nums: List[int], x: int) -> int:
        total = sum(nums)
        target = total - x
        sums = {0: -1}
        current, length = 0, float('-inf')
        
        if target == 0:
            return len(nums)
        
        for i, n in enumerate(nums):
            current += n
            
            if (current - target) in sums:
                length = max(length, i - sums[current - target])
            
            if current not in sums:
                sums[current] = i
        
        return (len(nums)-length) if length != float('-inf') else -1
        
```

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
https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/ <br />
https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/discuss/87768/4-lines-Python <br />
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

### Sliding Window

Cases:
1. Expand end pointer and update the start one according to the situation
2. Expand until the condition is met and then contract with the start one until the condition holds

https://leetcode.com/problems/longest-substring-without-repeating-characters/
```py
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        # extend as much we can, if we encounter a letter that falls inside the current
        # window, >= start, just update the start to start+1 otherwise just expand.
        # Update last seen with each iteration.
        # The window spans fron start to end, both inclusive
        last_seen = {}
        n = len(s)
        start = end = 0
        
        max_len = 0
        while end < n:
            if last_seen.get(s[end], -1) < start:
                # outside the window, no issues with expanding
                max_len = max(max_len, end - start + 1)
            else:
                # inside, update start
                start = last_seen[s[end]] + 1
            
            last_seen[s[end]] = end
            end += 1
        
        return max_len
```
https://leetcode.com/problems/minimum-size-subarray-sum/ <br />
Classic case 2 question. Expand until we meet the condition and then contract with start pointer forwarding.
```py
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        _sum, n = 0, len(nums)
        start = end = 0
        
        smallest_len = float('inf')
        
        while end < n:
            _sum += nums[end]
            
            if _sum >= target:
                while _sum >= target and start <= end:
                    _sum -= nums[start]
                    smallest_len = min(smallest_len, end - start + 1)
                    start += 1
            
            end += 1
        
        return smallest_len if smallest_len != float('inf') else 0
```
https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/ <br />
We just store the first occurance of the sum so that we always maximize the length.
```py
class Solution:
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        current_sum, max_len = 0, float('-inf')
        sum_store = {0: -1}
        
        for i, num in enumerate(nums):
            current_sum += num
            
            # current_sum - prev_sum = k => current_sum - k = prev_sum
            prev_sum = (current_sum - k)
            if prev_sum in sum_store:
                max_len = max(max_len, i-sum_store[prev_sum])
            
            # catch
            if current_sum not in sum_store:
                sum_store[current_sum] = i
        
        return max_len if max_len != float('-inf') else 0
```
https://leetcode.com/problems/minimum-window-substring/ <br />
We know the target dict. We can easily maintain current window with sliding window. <br />
For satisfy criteria, we need to know how many of the characters in target are in enough quantity. <br />
If we hit the target count for current character(not greater, that comes by default), we found a possible solution. <br />
Now contract the window (`start += 1`) until `found == required` character count
```py
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        """
        Algorithm:
        Expand the sliding window (with end += 1 ). In every loop, try to contract it 
        until it keeps on satisfying *(if it satifies)* the criteria (with start += 1). 
        Keep target and current dicts to update `found` variable in O(1).
        
        Optimizations:
        `found` variable to simulate O(1) time isConstraintSatified().
        Contract as long as this condition is satisfied.
        """
        
        start = end = 0
        target, current = {}, {}
        
        result, START, END = [float('-inf'), float('inf')], 0, 1
        
        # target set for constraint
        for c in t:
            target[c] = target.get(c, 0) + 1 
        
        found, required = 0, len(target)
        
        while end < len(s):
            char = s[end]
            
            # expand on the right side
            if char in target:
                current[char] = current.get(char, 0) + 1
                if current[char] == target[char]:
                    found += 1
            
            # contract until the condition is satisfied
            while found == required:
                # update result
                if (result[END] - result[START] + 1) > (end - start + 1):
                    result[START] = start
                    result[END] = end
                
                char = s[start]
                
                if char in current:
                    current[char] -= 1
                    if current[char] < target[char]:
                        found -= 1
                
                start += 1
               
            end += 1
            
        return s[result[START]: result[END]+1] if result[END] != float('inf') else ""
```
https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/ <br />
```py
class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        """
        Algo:
        Expand the window on the right side i.e. end. In each loop, run another loop to satisfy the constraint.
        We have contract on the start side, contract until the constraint is satisfied over the character store
        i.e. char_store

        Maintain current length and use that to update max_len.
        """
        start, end, n = 0, 0, len(s)
        chars, longest = {}, 0
        
        while end < n:
            char = s[end]
            chars[char] = chars.get(char, 0) + 1
            
            while len(chars) > k:
                start_char = s[start]
                chars[start_char] -= 1
                
                if chars[start_char] == 0:
                    del chars[start_char]
                
                start += 1
            
            longest = max(longest, end-start+1)
            end += 1
        
        return longest
```
https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/ <br />
`k == 2`
```py
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:
        start, end, n = 0, 0, len(s)
        chars, longest = {}, 0
        
        while end < n:
            char = s[end]
            chars[char] = chars.get(char, 0) + 1
            
            while len(chars) > 2:
                start_char = s[start]
                chars[start_char] -= 1
                
                if chars[start_char] == 0:
                    del chars[start_char]
                
                start += 1
            
            longest = max(longest, end-start+1)
            end += 1
        
        return longest
```
