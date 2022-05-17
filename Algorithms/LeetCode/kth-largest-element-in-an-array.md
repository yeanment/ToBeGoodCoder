## 215. Kth Largest Element in an Array

### Descirption 
Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

#### Constraints
- 1 <= k <= nums.length <= 10,000
- -10,000 <= nums[i] <= 10,000

### Sol 1: Binary Search + Insert

```C++
class Solution {
    int findPos(vector<int>& nums, int toInsert, int k)
    {
        if(nums.size() == 0)
            return 0;
        int l = 0;
        int r = (nums.size() > k? k: nums.size()) - 1;
        if(nums[r] > toInsert)
            return r + 1;
        while(l < r)
        {
            int m = l + (r - l)/2;
            if(nums[m] == toInsert)
                return m;
            else if(nums[m] < toInsert)
            {
                r = m;
            }
            else
            {
                l = m + 1;
            }
        }
        return r;
    }
public:
    int findKthLargest(vector<int>& nums, int k) {
        vector<int> ret(1, nums[0]);
        for(int i = 1; i < nums.size(); i++)
        {
            int idInsert = findPos(ret, nums[i], k);
            // cout << endl << idInsert << endl;
            // for(int j = 0; j < ret.size(); j++)
            //     cout<< ret[j] << ", ";
            if(idInsert >= 0 && idInsert < k)
                ret.insert(ret.begin() + idInsert, nums[i]);
        }
        return ret[k - 1];
    }
};
```
Note:
- Runtime: 40 ms
- Memory Usage: 10.3 MB

### Sol 2: Priority Queue

```C++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> ret;
        for(int i = 0; i < nums.size(); i++)
        {
            ret.push(nums[i]);
            if(ret.size() > k)
                ret.pop();            
        }
        return ret.top();
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 10.2 MB
