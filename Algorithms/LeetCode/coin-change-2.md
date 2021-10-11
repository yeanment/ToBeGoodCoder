## 518. Coin Charge 2

### Descirption 
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

#### Constraints
- 1 <= coins.length <= 300
- 1 <= coins[i] <= 5000
- All the values of coins are unique.
- 0 <= amount <= 5000

### Sol 1: using the depth-first search

```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int n = coins.size();
        int count = 0;
        vector<int> dpCoin(amount+1, 0);
        dpCoin[0] = 1;
        for(int i = 0; i < n; i++)
        {
            for(int j = 1; j <= amount; j++)
            {
                if(j >= coins[i])
                    dpCoin[j] = dpCoin[j] + dpCoin[j-coins[i]];
                // cout << dpCoin[j] << ' ';
            }
            // cout << endl;
        }
        return dpCoin[amount];
    }
};
```
Note:
- Runtime: 20 ms
- Memory Usage: 7.3 MB

**Further optimized**
```C++
#define fastIO ios::sync_with_stdio(0); cin.tie(0); cout.tie(0); 

class Solution {
public:
    int change(int amount, vector<int>& coins) {
    fastIO;
        int dp[amount + 1];
        memset(dp,0,sizeof(dp));
        dp[0] = 1;
        for (const auto & c : coins) 
            for (int i = c; i <= amount; ++i) 
                dp[i] += dp[i - c];    
        
        return dp[amount];
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7 MB
