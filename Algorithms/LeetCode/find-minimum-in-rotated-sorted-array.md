## 153. Find Minimum in Rotated Sorted Array

### Descirption 
Suppose an array of length n sorted in ascending order is **rotated** between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:
- [4,5,6,7,0,1,2] if it was rotated 4 times.
- [0,1,2,4,5,6,7] if it was rotated 7 times.

Notice that **rotating** an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of **unique** elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

#### Constraints
- n == nums.length
- 1 <= n <= 5000
- -5000 <= nums[i] <= 5000
- All the integers of nums are unique.
- nums is sorted and rotated between 1 and n times.


### Sol 1: Binary Search

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 1, h = nums.size() - 1;
        if(nums[0] <= nums[h]) return nums[0];
        while(l < h)
        {
            int mid = l + (h - l)/2;
            if(nums[mid] < nums[mid - 1])
                return nums[mid];
            else
            {
                if(nums[mid] > nums[h])
                    l = mid + 1;
                else 
                    h = mid - 1;
            }
        }
        return nums[l];
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 10.1 MB
