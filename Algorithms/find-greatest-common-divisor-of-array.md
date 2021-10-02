## 1979. Find Greatest Common Divisor of Array

### Descirption 
Given an integer array nums, return the greatest common divisor of the smallest number and largest number in nums.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

#### Constraints
- 2 <= nums.length <= 1000
- 1 <= nums[i] <= 1000

### Sol 1:

```C++
class Solution {
public:
    int gcd(int x, int y)
    {
        return y? gcd(y, x%y) : x;
    }
    
    int findGCD(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return gcd(nums[0], nums[nums.size()-1]);
    }
};
```
Note:
- Runtime: 9 ms
- Memory Usage: 12.5 MB

**Further optimized**
```C++
class Solution {
public:
    int findGCD(vector<int>& nums) {
        int smallest = *min_element(nums.begin(),nums.end());
        int largest = *max_element(nums.begin(),nums.end());
        return gcd(smallest,largest);
        }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 12.5 MB

