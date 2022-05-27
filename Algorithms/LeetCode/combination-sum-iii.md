## 216. Combination Sum III

### Descirption 
Find all valid combinations of k numbers that sum up to n such that the following conditions are true:
- Only numbers 1 through 9 are used.
- Each number is used at most once.

Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

#### Constraints
- 2 <= k <= 9
- 1 <= n <= 60


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(int k, int i, int target, vector<int>& path, vector<vector<int>>& result)
    {
        if(target == 0 && path.size() == k)
        {
            result.push_back(path);
            return;
        }
        if(i > 9 || target < 0 || i > target || path.size() > k)
        {
            return;
        }
        for(int ii = i; ii < 10; ii++)
        {
            path.push_back(ii);
            dfs(k, ii + 1, target - ii, path, result);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ret;
        vector<int> path;
        dfs(k, 1, n, path, ret);
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6.4 MB
