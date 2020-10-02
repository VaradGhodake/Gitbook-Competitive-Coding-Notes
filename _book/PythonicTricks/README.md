# Pythonic Tricks

INT_MAX:
```py
INT_MAX = float('inf')
```

INT_MIN:
```py
INT_MIN = float('-inf')
```

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
```
import heapq
heap = []
heapq.heappush(heap, (priority, item))
```

#### Efficient looping:
`s` can be a list or a string
```py
for i, c in enumerate(s):
    # i is index
    # c is s[i]
```

#### Memoization 
We can use `LRU cache` to store function results <br />
https://dbader.org/blog/python-memoization <br />
Use `functools.lru_cache` 
```py
import functools

@functools.lru_cache(maxsize=None)
def fib_lru_cache(n):
```

#### list.insert(index, elem)
The last one gets the priority