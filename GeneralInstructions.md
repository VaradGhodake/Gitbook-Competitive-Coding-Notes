# General instructions

#### Problem solving
* Brute-force approach first
* Keep on optimizing code
* Remember runtimes of python built-in functions and libraries

#### Code writing
* OOP: use `class`, `methods`, and class variables
* Use docstrings
* Helper functions to maintain some level of abstraction
* Property, getters-setters to decorate code further
* Use brackets where ever possible: avoids stupid mistakes and improves code readability

#### Python stuff
Everything in python is `pass-object reference by value`
> This question! => (https://leetcode.com/contest/weekly-contest-190/problems/pseudo-palindromic-paths-in-a-binary-tree/)

Create a copy of the parameter somehow. `list + [new]` works well for lists. <br />
For sets, `set - set([something]` creates a copy
