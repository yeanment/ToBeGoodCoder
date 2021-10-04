## 698. Partition to K Equal Sum Subsets

### Descirption 
Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

#### Constraints
- 1 <= k <= nums.length <= 16
- 1 <= nums[i] <= 104
- The frequency of each element is in the range [1, 4].

### Sol 1: using backtrack

```C++
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum = 0;
        sum = accumulate(nums.begin(), nums.end(), sum);
        if (nums.size() < k || sum % k) return false;
        
        vector<int> visited(nums.size(), false);
        return backtrack(nums, visited, sum / k, 0, 0, k);
    }
    
    bool backtrack(vector<int>& nums,vector<int>visited, int target, int curr_sum, int i, int k) {
        if (k == 0) 
            return true;
        
        if (curr_sum == target) 
            return backtrack(nums, visited, target, 0, 0, k-1);
        
        for (int j = i; j < nums.size(); j++) {
            if (visited[j] || curr_sum + nums[j] > target) continue;
            
            visited[j] = true;
            if (backtrack(nums, visited, target, curr_sum + nums[j], j+1, k)) return true;
            visited[j] = false;
        }
        
        return false;
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 16.6 MB

**Optimized for memory usage**
```C++
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        
        int sum = 0;
        sum = accumulate(nums.begin(), nums.end(), sum);
        
        if (nums.size() < k || sum % k) return false;
        
        vector<bool> seen(nums.size(),false);
        
        return backtrack(nums,sum/k,0,0,k,seen);
    }
    
    
    bool backtrack(vector<int>& nums, int target, int curr_sum, int index, int k, vector<bool> &seen){
        if(k == 1)
            return true;
        
        if(curr_sum > target)
            return false;
        
        if(target == curr_sum)
            return backtrack(nums,target,0,0,k-1,seen);
        
        for(int i = index; i < nums.size(); i++)
        {
            if(!seen[i])
            {
                seen[i] = true;                
                if(backtrack(nums,target, curr_sum+nums[i], i+1,k,seen))
                    return true;
                seen[i] = false;
            } 
        }
        return false;
    }
    
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 9.1 MB
