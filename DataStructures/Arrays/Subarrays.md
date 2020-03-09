
#### Subarrays
##### Optimization checks: <br />
1. Two pointers 
2. Sliding window with running values 
3. Prefix array: if we need to apply brute force at any point 

##### From all subarrays of size K:

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
