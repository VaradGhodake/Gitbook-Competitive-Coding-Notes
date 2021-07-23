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
https://leetcode.com/problems/merge-sorted-array/
```py
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        m_idx, n_idx, r_idx = m-1, n-1, len(nums1)-1
        
        while r_idx >= 0:
            # this is crucial; do not overflow n
            if n_idx < 0:
                break
            
            # first check is important to avoid m overflow
            if (m_idx >= 0) and nums1[m_idx] >= nums2[n_idx]:
                nums1[r_idx] = nums1[m_idx]
                m_idx -= 1
            else:
                nums1[r_idx] = nums2[n_idx]
                n_idx -= 1
            
            r_idx -= 1

```