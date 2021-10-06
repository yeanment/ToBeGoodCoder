## 34. Find First and Last Position of Element in Sorted Array

### Descirption 
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

#### Constraints
- 0 <= nums.length <= 100,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000
- nums is a non-decreasing array.
- -1,000,000,000 <= target <= 1,000,000,000

### Sol 1: Binary Search

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ret = {-1, -1};
        int n = nums.size();
        if(n == 0 || (n == 1 && nums[0] != target) || nums[0] > target || nums[n-1] < target) 
            return ret;
        int l = 0, h = n - 1;
        while(l < h)
        {
            int mid = l + (h - l)/2;
            cout << l << " : " << mid << " : " << h << endl;
            if(nums[mid] == target)
            {
                int lm = l, mm = mid, mh = h;
                cout << "lm: mm: mh" << endl;
                while(lm < mm)
                {
                    int lmm = lm + (mm - lm)/2;
                    cout << lm << " : " << lmm << " : " << mm << endl;

                    if(nums[lmm] == target)
                    {
                        mm = lmm;
                    }
                    else
                    {
                        lm = lmm + 1;
                    }
                }
                mm = mid;
                while(mm < mh)
                {
                    int mmh = mh - (mh - mm)/2;
                    cout << mm << " : " << mmh << " : " << mh << endl;
                    if(nums[mmh] == target)
                    {
                        mm = mmh;
                    }
                    else
                    {
                        mh = mmh - 1;
                    }
                }
                ret = {lm, mm};
                return ret;
            }
            else if(nums[mid] < target)
            {
                l = mid + 1;
            }
            else
            {
                h = mid - 1;
            }
        }
        if(nums[l] == target)
            ret = {l, h};
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 13.6 MB
