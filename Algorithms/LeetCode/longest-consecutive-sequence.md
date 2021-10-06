## 128. Longest Consecutive Sequence

### Descirption 
Given an unsorted array of integers nums, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in O(n) time.

#### Constraints
- 0 <= nums.length <= 100,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000

### Sol 1: 
```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int n = nums.size();
        if(n <= 1) 
            return n;
        sort(nums.begin(), nums.end());
        int ret = 0;
        int i = 0, j = i;
        while(i < nums.size() - 1 && j < nums.size())
        {
            j = i + 1;
            while( (nums[j] - nums[j - 1] <= 1))
            {                    
                j++;
                if(j == n)
                {
                    break;
                }
            }
            ret = max(ret, nums[j-1] - nums[i]+1);
            i = j;
        }
        return ret;
    }
};
```
Note:
- Runtime: 36 ms
- Memory Usage: 22.3 MB

