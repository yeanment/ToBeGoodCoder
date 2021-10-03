## 462. Minimum Moves to Equal Array Elements II

### Descirption 
Given an integer array nums of size n, return the minimum number of moves required to make all array elements equal.

In one move, you can increment or decrement an element of the array by 1.

Test cases are designed so that the answer will fit in a 32-bit integer.

#### Constraints
- n == nums.length
- 1 <= nums.length <= 100,000
- -1,000,000,000 <= nums[i] <= 1,000,000,000

### Sol 1:

```C++
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int count = 0;
        for(int i = 0; i < nums.size()/2; i++)
        {
            count += nums[nums.size() - 1 - i] - nums[i];
        }
        return count;
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 10.9 MB
