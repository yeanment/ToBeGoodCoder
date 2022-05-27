## 39. Combination Sum

### Descirption 
Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is guaranteed that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

#### Constraints
- 1 <= candidates.length <= 30
- 1 <= candidates[i] <= 200
- All elements of candidates are distinct.
- 1 <= target <= 500



### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(vector<int>& candidates, int i, int target, vector<int>& path, vector<vector<int>>& result)
    {
        if(target == 0)
        {
            result.push_back(path);
            return;
        }
        if(i >= candidates.size() || target < 0 || candidates[i] > target)
        {
            return;
        }
        for(int ii = i; ii < candidates.size(); ii++)
        {
            path.push_back(candidates[ii]);
            dfs(candidates, ii, target - candidates[ii], path, result);
            path.pop_back();
        }
    }


public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ret;
        vector<int> path;
        sort(candidates.begin(), candidates.end());
        dfs(candidates, 0, target, path, ret);
        return ret;
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 10.9 MB