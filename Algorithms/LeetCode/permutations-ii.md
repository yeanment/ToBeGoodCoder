## 47. Permutations II

### Descirption 
Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

#### Constraints
- 1 <= nums.length <= 8
- -10 <= nums[i] <= 10


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
                if(i > 0 && nums[i]==nums[i-1] && seen[i-1]) 
                    continue;
                path.push_back(nums[i]);
                seen[i] = true;
                dfs(nums, seen, path, result);
                seen[i] = false;
                path.pop_back();
            }
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<int> path;
        vector<vector<int>> ret;
        vector<bool> seen(nums.size(), false);
        dfs(nums, seen, path, ret);
        return ret; 
    }

};
```
Note:
- Runtime: 4 ms
- Memory Usage: 8.5 MB
