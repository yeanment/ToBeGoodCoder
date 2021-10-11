## 665. Non-decreasing Array

### Descirption 
Given an array nums with n integers, your task is to check if it could become non-decreasing by modifying at most one element.

We define an array is non-decreasing if nums[i] <= nums[i + 1] holds for every i (0-based) such that (0 <= i <= n - 2).

#### Constraints
- n == nums.length
- 1 <= n <= 10,000
- -100,000 <= nums[i] <= 100,000

### Sol 1:
```C++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int i = 2, ret = 1;
        if(nums.size() < 2)
            return true;
        if(nums[1] < nums[0])
        {
            nums[0] = nums[1];
            ret--;
        }
        while(i < nums.size() && ret >= 0)
        {
            if(nums[i] < nums[i-1])
            {
                if(nums[i-2] <= nums[i])
                {
                    nums[i-1] = nums[i-2];
                }
                else
                {
                    nums[i] = nums[i-1];
                }
                ret--;
            }
            else
            {
                i++;
            }
        }
        return ret>=0;
    }
};
```
Note:
- Runtime: 24 ms
- Memory Usage: 26.9 MB
