### Linked List

##### Sequence:

* https://leetcode.com/problems/delete-node-in-a-linked-list/
* https://leetcode.com/problems/middle-of-the-linked-list/
* https://leetcode.com/problems/remove-nth-node-from-end-of-list/
* https://leetcode.com/problems/reverse-linked-list/ _also practice recursive_
```
After swap, 
`prev` holds the value for result ie. head
`current` is None
```
* https://leetcode.com/problems/add-two-numbers/
* https://leetcode.com/problems/linked-list-cycle/

Tough ones:
* https://leetcode.com/problems/reverse-linked-list-ii
Adjusting connections after the reversal is probably the most crucial step <br />
```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        if not head or not head.next or m == n:
            return head
        
        # need these variables for post-reverse conenction adjustments
        node_before_reverse = m_reference = None
        prev = _next = None
        current = head
        
        count = 1
        
        while count <= n:
            if count < m:
                # before reverse adjustments
                node_before_reverse = current
                current = current.next
                count += 1
                continue
            
            if count == m:
                m_reference = current
            
            # actual reversal
            _next = current.next
            current.next = prev
            prev = current
            current = _next
            count += 1
        
        # connection adjustments
        if node_before_reverse:
            node_before_reverse.next = prev
        else:
            # head was one of the reverse nodes
            head = prev
        
        # this will always be required regardless of the fact that head was involved or not
        m_reference.next = current
        
        return head
```