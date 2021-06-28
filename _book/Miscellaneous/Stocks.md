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

##### Any number of transactions with transaction fee
> Can use this for any number of transactions with fee == 0 <br />
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/ <br />
At the end of the i-th day, we maintain cash, the maximum profit we could have if we did not have a share of stock, and hold, the maximum profit we could have if we owned a share of stock.

To transition from the i-th day to the i+1-th day, we either sell our stock <br />
`cash = max(cash, hold + prices[i] - fee`) or <br />
buy a stock <br />
`hold = max(hold, cash - prices[i])` <br />
At the end, we want to return cash. We can transform cash first without using temporary variables because selling and buying on the same day can't be better than just continuing to hold the stock.
```py
class Solution(object):
    def maxProfit(self, prices, fee):
        cash, hold = 0, -prices[0]
        for i in range(1, len(prices)):
            cash = max(cash, hold + prices[i] - fee)
            hold = max(hold, cash - prices[i])
        return cash
```

##### K number of transactions
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/discuss/54117/Clean-Java-DP-solution-with-comment <br />
Dynamic programming
