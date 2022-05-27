## 90. Subsets II

### Descirption 
Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Constraints
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(vector<int>& nums, int i, vector<int>& path, vector<bool>& seen, vector<vector<int>>& result)
    {
        if(path.size() <= nums.size() && path.size() > 0)
        {
            result.push_back(path);
            // return;
        }
        if(i >= nums.size())
        {
            return;
        }
        for(int ii = i; ii < nums.size(); ii++)
        {
            if(ii > 0 && nums[ii] == nums[ii-1] && seen[ii - 1] == false)
                continue;
            path.push_back(nums[ii]);
            seen[ii] = true;
            dfs(nums, ii + 1, path, seen, result);
            path.pop_back();
            seen[ii] = false;
        }
    }

public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> ret{vector<int> {}};
        sort(nums.begin(), nums.end());
        vector<int> path;
        vector<bool> seen(nums.size(), false);
        dfs(nums, 0, path, seen, ret);
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 7.5 MB
