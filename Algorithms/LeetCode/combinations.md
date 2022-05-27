## 77. Combinations

### Descirption 
Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].

You may return the answer in any order.

#### Constraints
- 1 <= n <= 20
- 1 <= k <= n


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(int i, int j, int& k, vector<int>& path, vector<vector<int>>& ret)
    {
        if(path.size() == k)
        {
            ret.push_back(path);
            return;
        }
        for(int ii = i + 1; ii <= j; ii++)
        {
            path.push_back(ii);
            dfs(ii, j, k, path, ret);
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ret;
        vector<int> path;
        dfs(0, n, k, path, ret);
        return ret;
    }
};
```
Note:
- Runtime: 30 ms
- Memory Usage: 9.1 MB
