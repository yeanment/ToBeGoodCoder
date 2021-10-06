## 1. Two Sum

### Descirption 
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

#### Constraints
- 2 <= nums.length <= 10,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000
- -1,000,000,000 <= target <= 1,000,000,000
- Only one valid answer exists.

### Sol 1: Search

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ret;
        for(int i = 0; i < nums.size(); i++)
        {
            int temp = target - nums[i];
            for(int j = i + 1;j < nums.size(); j++)
            {
                if(temp == nums[j]){
                    ret = {i, j};
                    return ret;
                }
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 176 ms
- Memory Usage: 10.2 MB


### Sol 2: Hash Search
```C++
class Solution {
public:
    vector<int> twoSum(vector<int> &nums, int target)
    {
        unordered_map<int, int> hash;
        vector<int> result;
        for (int i = 0; i < nums.size(); i++) {
            int numberToFind = target - nums[i];

            if (hash.find(numberToFind) != hash.end()) {
                result.push_back(hash[numberToFind]);
                result.push_back(i);			
                return result;
            }
            hash[nums[i]] = i;
        }
        return result;
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 12.4 MB