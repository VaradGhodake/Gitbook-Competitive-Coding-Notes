### Sorting

https://www.hackerrank.com/challenges/minimum-swaps-2/ <br />
Bring the expected number at the current index, not the other way around since we are looping over indices. <br />
Do not forget to update the original indices dict after swapping.
```py
# Complete the minimumSwaps function below.
def minimumSwaps(arr):
    indices = {v: k for k, v in enumerate(arr)}
    expected = sorted(arr)
    n, swaps = len(arr), 0
    
    for i in range(len(arr)):
        current = arr[i]
        
        if current != expected[i]:
            # expected number's id in arr
            expected_idx = indices[expected[i]]
            
            # swap: bring the expected number at i
            arr[i], arr[expected_idx] = arr[expected_idx], arr[i]
            
            # update indices hashmap
            indices[expected[i]] = i
            indices[current] = expected_idx
            
            # count this swap
            swaps += 1
    
    return swaps
```


#### Hashing

###### Freq of freq

* https://leetcode.com/problems/maximum-frequency-stack/ 

https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem?isFullScreen=false
```py
def isValid(s):
    freq = defaultdict(int)
    F = defaultdict(int)
    adjustment_options = 1
    
    for char in s:
        freq[char] += 1
    
    for _, v in freq.items():
        F[v] += 1
    
    if len(F) > 2:
        return "NO"
    
    if len(F) == 1:
        return "YES"
    
    max_key = max(F.keys())
    max_key_freq = F[max_key]
    min_key = min(F.keys())
    min_key_freq = F[min_key]
    
    return "YES" if ((max_key_freq == 1) and (max_key - min_key == 1)) or (min_key_freq == 1) else "NO"
```
