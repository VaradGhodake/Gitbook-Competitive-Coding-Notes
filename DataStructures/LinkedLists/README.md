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
https://leetcode.com/problems/intersection-of-two-linked-lists/ <br />
Great question. There's a similar question for Tree:LCA (`Tree/README.md`) <br />
```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if not headA or not headB:
            return None
        
        A, B = headA, headB
        resets = 2
        
        while not (headA is headB):
            if headA.next:
                headA = headA.next
            else:
                resets -= 1
                if resets < 0:
                    return None
                headA = B
            
            if headB.next:
                headB = headB.next
            else:
                resets -= 1
                if resets < 0:
                    return None
                headB = A
        
        return headA
```
https://leetcode.com/problems/remove-nth-node-from-end-of-list/ <br />
We need to reach to the `(n+1)th` from the last element => fast should run n first <br />
Edge case being `fast == last_node.next` ie. `None`
```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        fast = slow = head
        
        for _ in range(n):  
            fast = fast.next
        
        if fast is None:
            return head.next
        
        while fast.next:
            slow = slow.next
            fast = fast.next
        
        slow.next = slow.next.next
        return head
```