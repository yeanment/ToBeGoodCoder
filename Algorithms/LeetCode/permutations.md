## 102. Binary Tree Level Order Traversal

### Descirption 
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

#### Constraints
- 1 <= nums.length <= 6
- -10 <= nums[i] <= 10
- All the integers of nums are unique.


### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    void dfs(vector<int>& nums, vector<bool>& seen, vector<int>& path, vector<vector<int>>& result)
    {
        if(path.size() == nums.size())
        {
            result.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); i++)
        {
            if(seen[i] == false)
            {
                path.push_back(nums[i]);
                seen[i] = true;
                dfs(nums, seen, path, result);
                seen[i] = false;
                path.pop_back();
            }
        }
    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> path;
        vector<vector<int>> ret;
        vector<bool> seen(nums.size(), false);
        dfs(nums, seen, path, ret);
        return ret;    
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 7.6 MB
