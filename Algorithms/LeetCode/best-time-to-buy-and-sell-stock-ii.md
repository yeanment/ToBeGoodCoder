## 122. Best Time to Buy and Sell Stock II

### Descirption 
You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

#### Constraints
- 1 <= prices.length <= 30,000
- 0 <= prices[i] <= 10,000

### Sol 1: Binary Search

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ret = 0, minIn = prices[0];
        for(int i = 1; i < prices.size(); i++)
        {
            if(prices[i] < prices[i - 1])
            {
                ret += (prices[i-1] - minIn);
                minIn = prices[i];
            }
        }
        ret += (prices[prices.size() - 1]  - minIn);
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 13.1 MB
