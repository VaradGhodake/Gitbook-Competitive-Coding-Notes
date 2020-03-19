# DFS

Process current node and then process its children <br />
https://leetcode.com/problems/employee-importance/
```py
"""
# Employee info
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        # It's the unique id of each node.
        # unique id of this employee
        self.id = id
        # the importance value of this employee
        self.importance = importance
        # the id of direct subordinates
        self.subordinates = subordinates
"""
class Solution:
    
    def recursiveImportance(self, employee_data: dict, id: int) -> int:     
        current = employee_data[id]["importance"]
        
        for subordinate in employee_data[id]["subordinates"]:
            current += self.recursiveImportance(employee_data, subordinate)

        return current
        
        
    def getImportance(self, employees: List['Employee'], id: int) -> int:
        employee_data = {item.id : \
                        {'importance': item.importance, \
                         'subordinates': item.subordinates} \
                        for item in employees}
        
        return self.recursiveImportance(employee_data, id)
                        
```