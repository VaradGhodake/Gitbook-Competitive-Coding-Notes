### Linked List


#### Instructions
 <br />
 
* Important thing is to understand how long the loop should run. That is, until the end node or `None` after the end node. <br />
* Using multiple pointers is common, check if that helps. Also, be sure which one of these should be your point of reference. That might make a difference. (Check _reverse linked list_ question)  <br />
* To avoid many `if conditions`, you can declare a `None` node and use it as dummy or `prev` (merge sorted linked lists)
* Can treat a linked list as a graph: _linked list cycle_

Recursion gives us an elegant way to iterate through the nodes in reverse. For example, this algorithm will print out the values of the nodes in reverse. Given a node, the algorithm checks if it is null. If it is null, nothing happens. Otherwise, all nodes after it are processed, and then the value for the current node is printed.

```py
def print_values_in_reverse(ListNode head)
    if head is not None:
        print_values_in_reverse(head.next)
        print head.val
```

##### Sequence:

* https://leetcode.com/problems/delete-node-in-a-linked-list/
* https://leetcode.com/problems/middle-of-the-linked-list/

https://leetcode.com/problems/reverse-linked-list/ _also practice recursive_
```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev, current = None, head
        
        while current:
            # avoid reserved keyword, next.
            # loop runs till there is current, next = current.next 
            # in the prep phase can overflow. We shouldn't have a situation where
            # we are checking the next elem of None.
            _next = current.next
            
            # swap elements here
            current.next = prev
            
            # prepare for the next iteration
            prev = current
            current = _next
        
        return prev
```
Recursive approach: <br />
This is inspired from `tree -> doubly linked list` conversion question. <br />
With recursion, we get pointers in reverse order. Static variable to keep track of the previous nodes.
```py
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head:
            return head
        
        def helper(node):
            if not node:
                return
            
            # recursive call to reach the end 
            helper(node.next)
            
            if not helper.last:
                helper.first = node
            else:
                helper.last.next = node
                helper.last = node
            
            helper.last = node
        
        helper.first = helper.last = None
        helper(head)
        
        helper.last.next = None
        return helper.first
```
https://leetcode.com/problems/linked-list-cycle/ <br />
Iterative:
```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        visited = set()
        
        while head:
            fingerprint = id(head)
            if fingerprint in visited:
                return True
            
            visited.add(fingerprint)
            head = head.next
        
        return False
```
Recursive: <br />
Treat this as a graph problem asking for the cycle existance. We just need local visited set in this case. Also, there is not guaratee node values are different. We use their addresses to comapare (`id` function)
```py
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        visited = set()
        
        def traverse(node=head):
            if not node or not node.next:
                return False
            
            if id(node) in visited:
                return True
            
            visited.add(id(node))
            return traverse(node.next)
        
        return traverse()
```
https://leetcode.com/problems/merge-two-sorted-lists/
```py
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        # dummy node to instantiate things
        head = runner = ListNode()
        
        while l1 and l2:
            if l1.val <= l2.val:
                runner.next = l1
                l1 = l1.next
            else:
                runner.next = l2
                l2 = l2.next
            
            runner = runner.next
            
        runner.next = l1 or l2
        return head.next
```
https://leetcode.com/problems/merge-k-sorted-lists/ <br />
Great heap question.
```py
import heapq

class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        head = runner = ListNode()
        heap = []
        
        for i, _list in enumerate(lists):
            # important corner-case
            if _list is not None:
                heapq.heappush(heap, (_list.val, i))
        
        while heap:
            # we don't care about the value
            _, list_idx = heapq.heappop(heap)
            
            runner.next = lists[list_idx]
            
            runner = runner.next
            lists[list_idx] = lists[list_idx].next
            
            if lists[list_idx] is not None:
                # avoid pushing if the list at that index is None
                heapq.heappush(heap, (lists[list_idx].val, list_idx))
        
        return head.next
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
        """
        We need a pointer to the node before target (the one we've 
        been asked to remove).
        
        When the fast pointer reaches the end of the list, slow points
        to the target node. So, when slow points to the one just before
        that, fast one points to the last node that has 'next' ie. the 
        one before last.
        
        So, move the fast one n place and then move both until fast.next.
        Then perform deletion.
        """                                          
        fast = slow = head
        
        for _ in range(n):
            fast = fast.next
        
        if fast is None:
            # important edge case where
            # there are only n nodes in the list
            return head.next
        
        while fast.next:
            fast = fast.next
            slow = slow.next
        
        slow.next = slow.next.next
        return head
        
```
https://leetcode.com/problems/add-two-numbers/
```py
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = runner = ListNode()
        carry = 0
        
        while l1 or l2 or carry:
            l1_val = l1.val if l1 else 0
            l2_val = l2.val if l2 else 0
            
            _sum = (l1_val + l2_val + carry)
            digit = _sum % 10
            carry = _sum // 10
            
            runner.next = ListNode(digit)
            runner = runner.next
            
            if l1:
                l1 = l1.next
            
            if l2:
                l2 = l2.next            
        
        return dummy.next
```


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
