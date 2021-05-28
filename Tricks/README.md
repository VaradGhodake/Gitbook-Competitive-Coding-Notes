### Tricks

#### Prefix array and sum calculation
```py
        P = [0]
        
        for x in A: P.append(P[-1] + x)
        def sum_and_number_of_elements(x, y):
            # _IMP:_ x should be the start of slice+1; same for y
            return (P[j] - P[i]), (j - i)
```

#### Partitioning
1. Select last element as a `pivot` and first element as the `left_idx`
2. Add a `runner` to loop from left to (right-1)
3. `left_idx`to keep smaller elements at the correct places. Swap (left_idx - runner elems) if runner pointed elem is smaller than the pivot
```py
        def partition(_left, _right):
            left_idx, pivot_idx = _left, _right
            
            for runner in range(_left, _right):
                if nums[runner] < nums[pivot_idx]:
                    nums[left_idx], nums[runner] = nums[runner], nums[left_idx]
                    left_idx += 1
                
            
            nums[pivot_idx], nums[left_idx] = nums[left_idx], nums[pivot_idx]
            return left_idx
```

#### Rather than doing all sorts of complicated stuff with `// +- 1`, just use `math` <br />
```py
import math

math.ceil
math.floor
```

To compare references, do it with `is` <br />
`key` in `min/max/sorted` should get a function. You can create a lambda if required. <br />
Make a comparison function and use it with lambda. (check Multiple Arrays section)

#### Sort list with a tie-breaker
Example: https://leetcode.com/problems/remove-covered-intervals/
Also look at `tricky sorting` in Algorithms section
```py
sorted(list, key = lambda x: (x[0], -x[1]))
```
Second element is a tie-breaker. 

#### Convert list into a dictionary
```py
{item[0]: item[1:] for item in list}
```

#### Sort dictionary by value
```py
{k: v fo r k, v in sorted(x.items(), key=lambda item: item[1])}
```

#### Most frequent in an array or a string
```py
from collections import Counter
C = Counter(list/string)
C.most_common(n) #returns a list of top n
```
Or we can just keep track of the most frequent element while going throught the list

#### heapq insert with priority
```py
import heapq
heap = []
heapq.heappush(heap, (priority, item))
```

#### heapq also has a merge function
We can use this to merge sorted lists
```py
from heapq import merge
merge(list1, list2)
```

#### Efficient looping:
`s` can be a list or a string
```py
for i, c in enumerate(s):
    # i is index
    # c is s[i]
```

#### list.insert(index, elem)
The last one gets the priority

