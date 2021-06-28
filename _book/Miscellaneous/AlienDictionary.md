### Alien Dictionary

https://leetcode.com/problems/verifying-an-alien-dictionary/ <br />
Return `True` at the end, just look for an out-of-order case
```py
class Solution:
    def isAlienSorted(self, words: List[str], order: str) -> bool:
        seq = {c: i for i, c in enumerate(order)}
        
        for i, word in enumerate(words[:len(words)-1]):
            j = 0
            while j < min(len(word), len(words[i+1])):
                if seq[word[j]] > seq[words[i+1][j]]:
                    return False
                elif seq[word[j]] < seq[words[i+1][j]]:
                    break
                j += 1
            
            # we only hit len(one of the strings) if they have a common prefix
            # in that case, smaller one should be ahead of the bigger one
            if j == min(len(word), len(words[i+1])):
                if len(word) > len(words[i+1]):
                    return False
        
        return True
```
