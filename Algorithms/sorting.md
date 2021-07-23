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
