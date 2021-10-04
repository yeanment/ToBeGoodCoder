## 88. Merge Sorted Array

### Descirption 
You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

#### Constraints
- nums1.length == m + n
- nums2.length == n
- 0 <= m, n <= 200
- 1 <= m + n <= 200
- -1,000,000,000 <= nums1[i], nums2[j] <= 1,000,000,000

### Sol 1:

```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int idx = m + n -1;
        while(m > 0 || n > 0)
        {
            if(m > 0 && n > 0)
            {
                if(nums1[m - 1] > nums2[n - 1])
                {
                    nums1[idx--] = nums1[(m--)-1];
                }
                else
                {
                    nums1[idx--] = nums2[(n--)-1];
                }
            }
            else if(m == 0)
            {
                nums1[idx--] = nums2[(n--)-1];
            }
            else
                break;
        }
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 9.1 MB
