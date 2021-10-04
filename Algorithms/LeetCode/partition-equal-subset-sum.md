## 416. Partition Equal Subset Sum

### Descirption 
Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

#### Constraints
- 1 <= nums.length <= 200 
- 1 <= nums[i] <= 100

### Sol 1: using the depth-first search

```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        
        int sum = 0;
        
        for(vector<int>::iterator iter=nums.begin(); iter!=nums.end(); iter++)
            sum += *iter;
        if( sum%2 != 0)
            return false;
        int toPack = (int)sum/2;
        // Now is the kackpack problem
        int n = nums.size();
        vector<vector<int>> dpMatrix(n, vector<int>(toPack+1, 0));
        if(nums[0] < toPack)
            dpMatrix[0][nums[0]] = nums[0];
        for(int i=0; i<n; i++)
        {
            for(int j=1; j<=toPack;j++)
            {
                if(j >= nums[i] && i > 0)
                    dpMatrix[i][j] = max(dpMatrix[i-1][j], dpMatrix[i-1][j-nums[i]] + nums[i]);
                else if(j >= nums[i] && i == 0)
                    dpMatrix[i][j] = dpMatrix[0][nums[i]];
                else if (j < nums[i] && i > 0)
                    dpMatrix[i][j] = dpMatrix[i-1][j];
            }
        }
        // for(int i = 0; i < n; i++)
        // {
        //     for(int j = 0; j <= toPack; j++)
        //         cout << dpMatrix[i][j] << ' ';
        //     cout << endl;
        // }
        if(dpMatrix[n-1][toPack] == toPack)
            return true;
        return false;
    }
};
```
Note:
- Runtime: 300 ms
- Memory Usage: 75.3 MB

**Optimized for memory usage**
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        
        int sum = 0;
        
        for(vector<int>::iterator iter=nums.begin(); iter!=nums.end(); iter++)
            sum += *iter;
        if( sum%2 != 0)
            return false;
        int toPack = (int)sum/2;
        // Now is the kackpack problem
        int n = nums.size();
        vector<int> dpMatrix(toPack+1, 0);
        if(nums[0] < toPack)
            dpMatrix[nums[0]] = nums[0];
        for(int i=0; i<n; i++)
        {
            for(int j=toPack; j>=0;j--)
            {
                if(j >= nums[i] && i > 0)
                    dpMatrix[j] = max(dpMatrix[j], dpMatrix[j-nums[i]] + nums[i]);
                else if(j >= nums[i] && i == 0)
                    dpMatrix[j] = dpMatrix[nums[i]];
                else if (j < nums[i] && i > 0)
                    dpMatrix[j] = dpMatrix[j];
            }
        }
        if(dpMatrix[toPack] == toPack)
            return true;
        return false;
    }
};
```
Note:
- Runtime: 212 ms
- Memory Usage: 9.9 MB



### Sol 2  
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int sum = 0;
        
        for (int i = 0; i < n; i++) 
           sum += nums[i];
        
        if ((sum & 1) == 1) 
           return false;
        
        int M = sum / 2;
        
        bool dp[M + 1];
        memset(dp, false, sizeof(dp));
        dp[0] = true;
        
        for(int i=0;i<n;i++)
            for(int j=M;j>0;j--)
            {
                if(j>=nums[i])
                    dp[j] = dp[j] or dp[j-nums[i]];
            }        
        return dp[M];
    }
};
```
Note:
- Runtime: 76 ms
- Memory Usage: 9 MB
