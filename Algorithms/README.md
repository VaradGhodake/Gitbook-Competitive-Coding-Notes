# Algorithms

#### To Do:

Palindrome questions, Linked lists

Memoization to DP jump

String questions: Alien dictionary
Trie: https://leetcode.com/discuss/interview-question/643158/google-phone-faulty-keyboard
https://leetcode.com/discuss/interview-question/281470/


Kadane questions* <br />
May weekly contest <br />
K repetition maximum sum <br />

Paranthesis, stocks and intervals

Matrix traversal questions <br />
https://leetcode.com/contest/weekly-contest-180/problems/lucky-numbers-in-a-matrix <br />
Understand zip function

https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3328/ <br />
https://leetcode.com/problems/sliding-window-maximum/discuss/111560/Python-O(n)-solution-using-deque-with-comments

Design input class (Trees, LL especially) or think about inputs (arrays, graphs, etc) 

Habit of dict.get(key, default)

#### Binary search 

```py
def binarySearch(start, end):
    if end >= start:
        mid = start + (end - start) // 2
        if arr[mid] == k:
            return mid
        if arr[mid] > k:
            return binarySearch(start, mid - 1)
        if arr[mid] < k:
            return binarySearch(mid + 1, end)
    else:
        return -1
```