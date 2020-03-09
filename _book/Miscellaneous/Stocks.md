### Stocks

##### 1 transaction
https://www.geeksforgeeks.org/maximum-difference-between-two-elements/ <br />
Keep track of the minimum element found so far, calculate the difference between the current and minimum found so far. <br />
Update the maximum difference if necessary.

##### At most 2 transactions
https://www.geeksforgeeks.org/maximum-profit-by-buying-and-selling-a-share-at-most-twice/ <br />
A similar approach to one transaction limit, maintain the minimum till that element and similar for `i + 1` and till the end of the List.

##### Any number of transactions
https://www.geeksforgeeks.org/stock-buy-sell/ <br />
```
Local minima List[i] <= List[i + 1] 
Local maxima List[i] >= List[i + 1]
```
Buy at local minima and sell at local maxima

##### K number of transactions
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/discuss/54117/Clean-Java-DP-solution-with-comment <br />
Dynamic programming
