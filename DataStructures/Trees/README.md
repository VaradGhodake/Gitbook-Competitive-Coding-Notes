# Trees

Decent curated list: https://medium.com/@codingfreak/binary-tree-interview-questions-and-practice-problems-439df7e5ea1f
#### Traversals
* Inorder (Recursive and Iterative)
* Preorder (Recursive and Iterative)
* Postorder (Recursive and Iterative)
* Levelwise
* Vertical order (Levelwise + Hashing)

#### Finding a solution
* See if you want to pass data down the tree or up the tree
    * If up the tree, postorder for recursive solution.
    * If down the tree, preorder for recursive solution, keep `globals` to record and update answer.  `return` for no node/ no left/ no right tree cases. If you don't want to encounter no left or no right situations, check before passing down the value.

* For levelwise, make sure you check if the left node or right node is present before pushing to the queue. Do it level by level not node by node. `popleft` and `append` combo (`collections.deque`)

### `Gotcha` questions
https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii/ <br />
Based on a similar question for linked lists (check if they merge or not) <br />
The trick is to eliminate distance discrepancy.

1. Go up and up through `parent` pointer
2. If one of them hits root (no parent), reset it to the other node and do the same thing again, this will remove distance discrepancy
3. If they are equidistant from the LCA (step 2 takes care of it if there are not), they'll converge there

```py

class Solution:
    def lowestCommonAncestor(self, p: 'Node', q: 'Node') -> 'Node':
        p1, q1 = p, q
        
        while p1 != q1:
            p1 = p1.parent if p1.parent else q
            q1 = q1.parent if q1.parent else p
        
        return p1
```
