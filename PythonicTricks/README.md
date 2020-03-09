# Pythonic Tricks

#### Sort dictionary by value
```py
{k: v for k, v in sorted(x.items(), key=lambda item: item[1])}
```

#### Most frequent in an array or a string
```py
from collections import Counter
C = Counter(list/string)
C.most_common(n) #returns a list of top n
```