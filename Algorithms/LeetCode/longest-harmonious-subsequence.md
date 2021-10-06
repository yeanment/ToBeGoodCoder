## 594. Longest Harmonious Subsequence

### Descirption 
We define a harmonious array as an array where the difference between its maximum value and its minimum value is exactly 1.

Given an integer array nums, return the length of its longest harmonious subsequence among all its possible subsequences.

A subsequence of array is a sequence that can be derived from the array by deleting some or no elements without changing the order of the remaining elements.

#### Constraints
- 1 <= nums.length <= 20,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000

### Sol 1: 

```C++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        int ret = 0;
        map<int, int> frequency;
        for(int i = 0; i < nums.size(); i++)
        {
            frequency[nums[i]]++;
        }
        for (map<int, int>::iterator  it=frequency.begin(); it!=frequency.end(); ++it)
        {
            map<int, int>::iterator itR = frequency.find(it->first + 1);
            if(itR != frequency.end())
            {
                int len = itR->second + it->second;
                ret = ret>len? ret:len;
            }
            
        }
        return ret;
    }
};
```
Note:
- Runtime: 100 ms
- Memory Usage: 41.8 MB
### Sol 2: Using sort
```C++
class Solution {
public:
    int findLHS(vector<int>& nums) 
    {
        int len=0;
        sort(nums.begin(),nums.end());
        int n=nums.size();
        int i1=0,i2=0;
        while((i1<n)&&(i2<n))
        {
            while((i2<n)&&(nums[i2]-nums[i1]<=1))
                i2++;
            if(nums[i2-1]==nums[i1]+1)
                len = max(len,i2-i1);
            i1++;
        }    
        return len;
    }
};
```
Note:
- Runtime: 52 ms
- Memory Usage: 32.5 MB