## 78. Subsets

### Descirption 
Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Constraints
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
- All the numbers of nums are unique.


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(vector<int>& nums, int i, vector<int>& path, vector<vector<int>>& result)
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
            path.push_back(nums[ii]);
            dfs(nums, ii + 1, path, result);
            path.pop_back();
        }
    }

public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ret{vector<int> {}};
        vector<int> path;
        dfs(nums, 0, path, ret);
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 7 MB
