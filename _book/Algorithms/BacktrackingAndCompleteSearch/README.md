# Backtracking and Complete Search

#### Subsets
Reference: CC Cheat sheet book <br />
Note the number of recursive calls made

Two choices:
* Don't choose the element: Just make a recursive call 
* Choose the element
    1. Select step
    2. Go to the next depth
    3. Pop out so that we don't have to instantiate the bucket

```
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


#### Permutations

Recursive calls for every element in the loop <br />
No need to add a ‘reject’ scenario <br />
Accept, make changes and revert after going a level deep <br />
Filter right after the loop using a helper array used for tracking <br />

```
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

