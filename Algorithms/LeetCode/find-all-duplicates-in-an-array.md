## 442. Find All Duplicates in an Array

### Descirption 
Given an integer array nums of length n where all the integers of nums are in the range [1, n] and each integer appears once or twice, return an array of all the integers that appears twice.

You must write an algorithm that runs in O(n) time and uses only constant extra space.

#### Constraints
- n == nums.length
- 1 <= n <= 105
- 1 <= nums[i] <= n
- Each element in nums appears once or twice.

### Sol 1: Using Set or BitSet

```C++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> ret;
        set<int> unique;
        for(int i = 0; i < nums.size(); i++)
        {
            if(unique.emplace(nums[i]).second == false)
                ret.push_back(nums[i]);
        }
        return ret;
    }
};
```
Note:
- Runtime: 177 ms
- Memory Usage: 52.7 MB

```C++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<bool> seen(nums.size(), false);
        
        vector<int> ret;
        for(int i = 0; i < nums.size(); i++)
        {
            if(seen[nums[i]])
                ret.push_back(nums[i]);  
            else
                seen[nums[i]] = true;
        }
        return ret;
    }
};
```
Note:
- Runtime: 58 ms
- Memory Usage: 33.8 MB

### Sol 2: Without extra space

```C++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
        vector<int> ret;
        for(int i = 0; i < nums.size(); i++)
        {
            int indexOfElem = abs(nums[i]) - 1;
            // negate the element
            nums[indexOfElem] = - nums[indexOfElem];
            // if this element is positive means double negation had happened which 
            // means we editied this element twice.
            if(nums[indexOfElem]>0)
                ret.push_back(indexOfElem+1);
        }
        return ret;
    }    
};
```
Note:
- Runtime: 100 ms
- Memory Usage: 33.5 MB

