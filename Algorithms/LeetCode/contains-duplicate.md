## 217. Contains Duplicate

### Descirption 
Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

#### Constraints
- 1 <= nums.length <= 100,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000

### Sol 1: 

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set<int> unique;
        for(int i = 0; i < nums.size(); i++)
        {
            if(!(unique.emplace(nums[i]).second))
                return true;
        }
        return false;
    }
};
```
Note:
- Runtime: 36 ms
- Memory Usage: 21 MB

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        return set<int>(nums.begin(), nums.end()).size() < nums.size();
    }
};
```
Note:
- Runtime: 40 ms
- Memory Usage: 22.6 MB

### Sol 2: Sort and Search
```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for(int i = 0 ; i < nums.size()-1 ; i++)
            if(nums[i] == nums[i+1])return true;
        return false;
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 15.6 MB