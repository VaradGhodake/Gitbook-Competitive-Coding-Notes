# Divide and Conquer 

eg. Kth smallest, LCA

Remember:
* In-order of binary gives a sorted list
* Result from left and right -> need to be taken care of
* Recursive calls just call left and right. They do not traverse the whole depth in one go. The basic principle of recursion.
* Look for the Pivot (distinguishing characteristics of that particular point)

The general solution template:
```
class solution:
	def __init__(self):
		#some props that we need pass down the tree
		#some props that we want to maintain values of

	def actual_solver(self, root):
		#if some props needed to be passed per node, use helper here
		#additional param needs to be passed for the helper
		If root == None:
			return
		left_solution  = actual_solver(root.left)

		#Node processing using global/local values
		#updation of those global/local values
		#if found:
		return answer
		
		Right_solution = actual_solver(root.right)
		#return answer based on left_solution and right_solution
		#imagine the stack after recursion
```	