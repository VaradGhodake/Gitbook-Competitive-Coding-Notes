# Algorithms

#### To Do:
1. Matrix traversal questions
https://leetcode.com/contest/weekly-contest-180/problems/lucky-numbers-in-a-matrix

2. Tree traversal questions

3. Priority queues and heapq python implementation

4. Next greater, next smaller, greatest rightmost, smallest rightmost in an array

5. BFS questions


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