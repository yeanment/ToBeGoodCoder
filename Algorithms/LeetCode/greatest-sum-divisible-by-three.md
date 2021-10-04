## 1262. Greatest Sum Divisible by Three

### Descirption 
Given an array nums of integers, we need to find the maximum possible sum of elements of the array such that it is divisible by three.

#### Constraints
- 1 <= nums.length <= 40000
- 1 <= nums[i] <= 10000

### Sol 1: with recursive function

```C++
class Solution {
public:
    int maxSumDivThreeMod(vector<int>& nums, int idx, int mod)
    {
        if(idx == 1)
        {
            return nums[idx - 1]% 3 == mod? nums[idx - 1] : (mod == 0? 0:-1);
        }
        int retCode0 = maxSumDivThreeMod(nums, idx - 1, (mod + 3 - nums[idx-1]%3)%3);
        int retCode1 = max(maxSumDivThreeMod(nums, idx - 1, mod), retCode0 == -1? -1:retCode0+nums[idx-1]);
        return (retCode1 == -1 ? -1:retCode1);
    }
    
    int maxSumDivThree(vector<int>& nums) {
        int retCode = maxSumDivThreeMod(nums, nums.size(), 0);
        return (retCode==-1? 0:retCode);
    }
};
```
Note:
- Runtime: Time Limited Exceeded

**Further optimized**
```C++
class Solution {
public:
    int maxSumDivThree(vector<int>& nums) {        
        vector<int> dp(3, 0);
        for(int i = 0; i < nums.size(); i++)
        {
            int a = nums[i] + dp[0];
            int b = nums[i] + dp[1];
            int c = nums[i] + dp[2];
            dp[a%3] = max(dp[a%3],a);
            dp[b%3] = max(dp[b%3],b);
            dp[c%3] = max(dp[c%3],c);
        }
        return dp[0];
    }
};
```
Note:
- Runtime: 28 ms
- Memory Usage: 24.9 MB

