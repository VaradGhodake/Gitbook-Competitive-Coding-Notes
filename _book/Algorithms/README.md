# Algorithms

#### To Do:

1. Palindrome questions

2. String questions: Alien dictionary

3. Kadane questions* <br />
May weekly contest <br />
K repetition maximum sum <br />

4. Paranthesis, stocks and intervals

5. Matrix traversal questions <br />
https://leetcode.com/contest/weekly-contest-180/problems/lucky-numbers-in-a-matrix <br />
Understand zip function

6. 
https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/535/week-2-may-8th-may-14th/3328/ <br />
https://leetcode.com/problems/sliding-window-maximum/discuss/111560/Python-O(n)-solution-using-deque-with-comments

7. Next greater, next smaller, greatest rightmost, smallest rightmost in an array


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