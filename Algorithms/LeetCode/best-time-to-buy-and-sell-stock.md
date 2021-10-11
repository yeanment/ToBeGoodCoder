## 121. Best Time to Buy and Sell Stock

### Descirption 
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

#### Constraints
- 1 <= prices.length <= 100,000
- 0 <= prices[i] <= 10,000

### Sol 1: 

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxPro = 0;
        int minPrice = numeric_limits<int>::max();
        for(int i = 0; i < prices.size(); i++){
            minPrice = min(minPrice, prices[i]);
            maxPro = max(maxPro, prices[i] - minPrice);
        }
        return maxPro;
    }
};
```
Note:
- Runtime: 120 ms
- Memory Usage: 93.3 MB
