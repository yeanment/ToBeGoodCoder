## 53. Maximum Subarray

### Descirption 
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.

#### Constraints
- 1 <= nums.length <= 100,000
- -10,000 <= nums[i] <= 10,000

### Sol 1: Kadane's algorithm
Kadane's algorithm begins with a simple inductive question: if we know the maximum subarray sum ending at position i, what is the maximum subarray sum ending at position i+1? The answer turns out to be relatively straightforward: either the maximum subarray sum ending at position i+1 includes the maximum subarray sum ending at position i as a prefix, or it doesn't. Thus, we can compute the maximum subarray sum ending at position i for all positions i by iterating once over the array. As we go, we simply keep track of the maximum sum we've ever seen. 
```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0], ret = nums[0];
        for(int i = 1; i < nums.size(); i++)
        {
            ret = ret > 0 ? ret + nums[i] : nums[i];
            maxSum = max(maxSum, ret);
        }
        return maxSum;
    }
};
```
Note:
- Runtime: 125 ms
- Memory Usage: 67.8 MB
