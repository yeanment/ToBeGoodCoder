## 540. Single Element in a Sorted Array

### Descirption 
You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return the single element that appears only once.

Your solution must run in O(log n) time and O(1) space.

#### Constraints
- 1 <= nums.length <= 100,000
- 0 <= nums[i] <= 100,000

### Sol 1: Binary Search

```C++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int n = nums.size();
        if(n == 1)
            return nums[0];
        int l = 0, h = nums.size() - 1;
        while(l < h)
        {
            int mid = l + (h - l)/2;
            // cout <<　l << " : " << mid << " : " << h << endl;
            if( mid&1 )
            {
                if(nums[mid] == nums[mid - 1])
                    l = mid + 1;
                else
                    h = mid - 1;
            }
            else
            {
                if(nums[mid] == nums[mid + 1])
                    l = mid + 2;
                else
                    h = mid;
            }
            // cout <<　l << " : " << mid << " : " << h << endl;
        }
        
        return nums[l];
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 11 MB
