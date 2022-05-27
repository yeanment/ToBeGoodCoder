## 40. Combination Sum II

### Descirption 
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

#### Constraints
- 1 <= candidates.length <= 100
- 1 <= candidates[i] <= 50
- 1 <= target <= 30


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(vector<int>& candidates, int i, int target, vector<bool>& seen, vector<int>& path, vector<vector<int>>& result)
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
            if(ii > 0 && candidates[ii] == candidates[ii - 1] && seen[ii-1] == false)
                continue;
            path.push_back(candidates[ii]);
            seen[ii] = true;
            dfs(candidates, ii + 1, target - candidates[ii], seen, path, result);
            path.pop_back();
            seen[ii] = false;
        }
    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<vector<int>> ret;
        vector<int> path;
        vector<bool> seen(candidates.size(), false);
        sort(candidates.begin(), candidates.end());
        dfs(candidates, 0, target, seen, path, ret);
        return ret;
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 10.4 MB
