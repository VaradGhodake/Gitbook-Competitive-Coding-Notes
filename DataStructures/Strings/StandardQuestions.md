# Standard Questions

#### Brute -> Sliding Window -> Set
https://leetcode.com/problems/longest-substring-without-repeating-characters/


#### Subsequence
https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/ <br />
Visualize the problem. Occurence of each letter in `s` should be after the last one and the last occurence of the same letter (we store these in `offsets` dictionary) <br />
If the number of matched letters is the same as the length of the word, we compare with the result. <br />
Peep the `min` function with `key=lambda`
```py
class Solution:
    def findLongestWord(self, s: str, d: List[str]) -> str:
        store = {}
        result = ''
        for i, c in enumerate(s):
            store[c] = store.get(c, []) + [i]
        
        for word in d:
            offsets = {}
            last = -1
            matched = 0
            
            for c in word:
                if c not in store:
                    break
                
                offset = offsets.get(c, 0)
                
                if offset == len(store[c]):
                    break
                
                for i in range(offset, len(store[c])):
                    if store[c][i] > last:
                        offsets[c] = i + 1
                        last = store[c][i]
                        matched += 1
                        break
            
            if matched == len(word):
                result = min(result, word, key= lambda x: (-len(x), x))
        
        return result
```


#### string rotation
https://leetcode.com/problems/group-shifted-strings/ <br />
`% 26` for circular rotation
```py
class Solution:
    def groupStrings(self, strings: List[str]) -> List[List[str]]:
        fingerprints = {}
        
        for string in strings:
            fingerprint = ''
            for i, c in enumerate(string[1:], start=1):
                diff = (ord(string[i-1]) - ord(c))
                fingerprint += str(diff % 26)
            
            fingerprints[fingerprint] = fingerprints.get(fingerprint, []) + [string]
        
        return fingerprints.values()
```
https://leetcode.com/problems/count-binary-substrings/ <br />
Modelling question. Keep it simple. Run the loop from second elem so the current and prev handling is easier.
```py
class Solution:
    def countBinarySubstrings(self, s: str) -> int:
        prev_count = 0 
        
        current = s[0]
        current_count = 1
        
        total = 0
        
        for c in s[1:]:
            if c == current:
                current_count += 1
            else:
                current = c
                prev_count = current_count
                current_count = 1    
            
            if current_count <= prev_count:
                total += 1
        
        return total
```