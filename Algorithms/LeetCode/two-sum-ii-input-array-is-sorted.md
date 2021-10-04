## 167. Two Sum II - Input array is sorted

### Descirption
Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= first < second <= numbers.length.

Return the indices of the two numbers, index1 and index2, as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

#### Constraints
- 2 <= numbers.length <= 30,000
- -1,000 <= numbers[i] <= 1,000
- numbers is sorted in non-decreasing order.
- -1,000 <= target <= 1,000
- The tests are generated such that there is exactly one solution.

### Sol 1: using the brute force search

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        vector<int> ret;
        for(int i = 0, j = numbers.size()-1; i < j; )
        {
            int sum = numbers[i] + numbers[j];
            if(sum == target)
            {
                ret = {i+1, j+1};
                return ret;
            }
            else if(sum > target)
                j--;
            else 
                i++;
        }
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 9.6 MB
