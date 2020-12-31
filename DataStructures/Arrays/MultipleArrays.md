### Multiple arrays

*NOTE:* <br />
`key` in `min/max/sorted` should get a function. You can create a lambda if required. <br />
Make a comparison function and use it with lambda.

https://leetcode.com/problems/intersection-of-three-sorted-arrays/ <br />
We try to maintain a state where all pointers point to the minimum of each array; if all equal, push it to result otherwise forward the pointer that points to the min of 3.
```py
class Solution:
    def arraysIntersection(self, arr1: List[int], arr2: List[int], arr3: List[int]) -> List[int]:
        index = {'i': 0, 'j': 0, 'k': 0}
        result = []
        
        def get_smallest(x):
            if x == 'i':
                return arr1[index[x]]
            if x == 'j':
                return arr2[index[x]]
            if x == 'k':
                return arr3[index[x]]
        
        while True:
            if arr1[index['i']] == arr2[index['j']] == arr3[index['k']]:
                result.append(arr1[index['i']])
                for i in index:
                    index[i] += 1
            else:
                index[min(index, key=lambda x: get_smallest(x))] += 1
            
            if index['i'] >= len(arr1) \
               or index['j'] >= len(arr2) \
               or index['k'] >= len(arr3):
                break
        
        return result
```