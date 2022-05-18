## 123. Best Time to Buy and Sell Stock III

### Descirption 
You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete at most two transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

#### Constraints
- 1 <= prices.length <= 100,000
- 0 <= prices[i] <= 100,000

### Sol 1: Binary Search

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int ret = 0, retL = 0;
        vector<int> retR(n + 1, 0);
        int bp = prices[0], hs = prices[n - 1];
        for(int i = n - 1; i >= 0; i--)
        {
            retR[i] = max(retR[i+1], hs - prices[i]);
            if(prices[i] > hs)
                hs = prices[i];
        }
        ret = retR[0];
        for(int i = 0; i < n; i++)
        {
            retL = max(retL, prices[i] - bp);
            if(prices[i] < bp)
                bp = prices[i];
            ret = max(ret, retL + retR[i + 1]);
        }
        return ret;
    }
};
```
Note:
- Runtime: 179 ms
- Memory Usage: 78.3 MB

### Sol 2:

```C++
// The magic trick here.
static const auto f = []() {
    std::ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    return 0;
}();


class Solution {
public:
    int maxProfit(vector<int>& prices) 
    {
        if(!prices.size())
            return 0;
        
        int buy1    = INT_MAX;
        int profit1 = INT_MIN;
        int buy2    = INT_MAX;
        int profit2 = INT_MIN;
        
        for(int i = 0; i < prices.size(); i++)
        {
            buy1    = min(buy1, prices[i]);
            profit1 = max(profit1, prices[i] - buy1);
            buy2    = min(buy2, prices[i] - profit1);
            profit2 = max(profit2, prices[i] - buy2);
        }
        
        return profit2;
    }
};
```
Note:
- Runtime: 179 ms
- Memory Usage: 78.3 MB
