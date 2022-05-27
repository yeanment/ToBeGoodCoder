## 474. Ones and Zeroes

### Descirption 
You are given an array of binary strings strs and two integers m and n.

Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

A set x is a subset of a set y if all elements of x are also elements of y.

#### Constraints
- 1 <= strs.length <= 600
- 1 <= strs[i].length <= 100
- strs[i] consists only of digits '0' and '1'.
- 1 <= m, n <= 100


### Sol 1: DFS

```C++
class Solution {
private:
    void dfs(vector<int>& num0, vector<int>& num1, int len, int i, int m, int n, int& maxsize)
    {
        if(m >= 0 && n >= 0)
        {
            maxsize = max(maxsize, len);
        }
        if(i > num0.size())
            return;
        for(int ii = i; ii < num0.size(); ii++)
        {
            dfs(num0, num1, len+1, ii+1, m - num0[ii], n - num1[ii], maxsize);
        }
    }

public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        // DFS or BFS
        vector<int> num0(strs.size(), 0);
        vector<int> num1(strs.size(), 0);
        int ret = 0;
        for(int i = 0; i < strs.size(); i++)
        {
            for(int j = 0; j < strs[i].length(); j++)
            {
                if(strs[i][j] == '0')
                    num0[i]++;
                else if(strs[i][j] == '1')
                    num1[i]++;
            }
        }
        dfs(num0, num1, 0, 0, m, n, ret);
        return ret;
    }
};
```
Note:
- Runtime: TLE
- Memory Usage: 

### Sol 2: DP with tab

```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        
        for(auto& s : strs) { 
            
            int zeros = count(s.begin(), s.end(), '0');
            int ones = s.size();
            
            for(int i = m; i >= zeros; i--) 
                for(int j = n; j >= (ones - zeros); j--) 
                    dp[i][j] = max(dp[i][j], 1 + dp[i - zeros][j - ones + zeros]);
        }
        
        return dp[m][n];
    }
};
```
Note:
- Runtime: 361 ms
- Memory Usage: 9.7 MB