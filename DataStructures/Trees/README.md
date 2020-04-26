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
