### Combinatorics

#### Subsets
Reference: CC Cheat sheet book <br />
Note the number of recursive calls made

Two choices:
* Don't choose the element: Just make a recursive call 
* Choose the element
    1. Select step
    2. Go to the next depth
    3. Pop out so that we don't have to instantiate the bucket

```py
class SolutionGenerator:
    def solution_search(self, k : int, domain: list) -> None:
        if (k == self.n):
            self.powerset.append(self.subset[:])
            return
        self.solution_search(k + 1, domain)
        self.subset.append(domain[k])
        self.solution_search(k + 1, domain)
        self.subset.pop()

    def _helper_generator(self, domain: list) -> list:
        self.powerset = []
        self.subset = []
        self.n = len(domain)
        self.solution_search(0, domain)
        return self.powerset
```


#### Combinations: select r out of n (nCr)

* Recursion base case change (==r)
* Params of recursion: s: number of selected so far; k: array item index to be processed in that call
* Similar to normal combinations


```py
class T:
    def normalSubset(self, arr: list, r: int) -> list:
        nCr = []
        bucket = []
        n = len(arr)
        
        def helperNCR(s: int, k: int) -> None:
            if(s == r):
                nCr.append(bucket[:])
                return
            
            if(s > r or k == n):
                return
            
            helperNCR(s, k + 1)
            bucket.append(arr[k])
            helperNCR(s + 1, k + 1)
            bucket.pop()
        
        helperNCR(0, 0)
        return nCr```

#### Permutations

Recursive calls for every element in the loop <br />
No need to add a ‘reject’ scenario <br />
Accept, make changes and revert after going a level deep <br />
Filter right after the loop using a helper array used for tracking <br />

```py
class Permutation:
    def perm_recur(self, k : int, domain: list) -> None:
        if(k == self.n):
            self.permutations.append(self.bucket[:])
            return
        for i in range(0, self.n):
            if(self.accepted[i]):
                continue
            self.accepted[i] = True
            self.bucket.append(domain[i])
            self.perm_recur(k + 1, domain)
            self.accepted[i] = False
            self.bucket.pop()

    def _helper(self, domain: list) -> list:
        self.n = len(domain)
        self.accepted = [False] * self.n
        self.permutations = []
        self.bucket = []
        self.perm_recur(0, domain)
        return self.permutations

```

Additional questions:
https://leetcode.com/problems/combination-sum/discuss/16510/Python-dfs-solution.

To allow repetition, go to the same node after select. <br />
```self.recurse(k, domain)```


#### Permutation over a limited set of values (with repetition)

* Select one option and de-select it
* Similar for other options 
* There are finite number of options at each step
* Recurse based on your selection

https://leetcode.com/problems/generate-parentheses/

```py
class Solution:
    def generateParenthesis(self, n: int) -> int:
        def backtracker(bucket: list, openCount: int) -> None:
            if((len(bucket) - openCount) > openCount or \
               openCount > self.n):
                return
            
            if(len(bucket) == self.n * 2):
                if(openCount == self.n):
                    self.permutations.append(''.join(bucket))
                return
            
            bucket.append('(')
            backtracker(bucket, openCount + 1)
            bucket.pop()
            bucket.append(')')
            backtracker(bucket, openCount)
            bucket.pop()

        self.permutations = []
        self.n = n
        if n == 0:
            return 0

        backtracker([], 0)
        return self.permutations
```