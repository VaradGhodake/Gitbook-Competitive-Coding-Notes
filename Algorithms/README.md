# Algorithms

#### To Do:

Runtimes: why heap is O(n)?

Addition questions-> strings, lists, *linked lists*

Factors/divisors questions

stocks

Bitwise

Palindrome questions, Linked lists

Memoization to DP jump

String questions: Alien dictionary
Trie: https://leetcode.com/discuss/interview-question/643158/google-phone-faulty-keyboard
https://leetcode.com/discuss/interview-question/281470/


Kadane questions* <br />
May weekly contest <br />
K repetition maximum sum <br />

Matrix traversal questions <br />
https://leetcode.com/contest/weekly-contest-180/problems/lucky-numbers-in-a-matrix <br />
Understand zip function <br />
spiral order

https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3328/ <br />

Design input class (Trees, LL especially) or think about inputs (arrays, graphs, etc) 

Habit of dict.get(key, default)

Streaming data questions: https://www.geeksforgeeks.org/tag/array-stream/

#### Case questions
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
