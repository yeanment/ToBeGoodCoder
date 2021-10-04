## 238. Product of Array Except Self

### Descirption 
Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i].

The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

#### Constraints
- 2 <= nums.length <= 100,000
- -30 <= nums[i] <= 30
- The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.

### Sol 1:

```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int product = 1;
        int cntZero = 0, idZero;
        vector<int> ret(nums.size(), 0);
        for(int i = 0; i < nums.size(); i++)
        {
            if(nums[i] == 0)
            {
                idZero = i;
                cntZero++;
                if(cntZero > 1)
                    return ret;
            }
            else
                product *= nums[i];
        }
        if(cntZero == 1)
            ret[idZero] = product;
        else
        {
            for(int i = 0; i < nums.size(); i++)
            {
                ret[i] = product/nums[i];
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 24 ms
- Memory Usage: 23.9 MB

### Sol 2:
```C++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> lhs(nums.size(), 1);
        int rhs = 1;
        for(int i = 1; i<nums.size() ; i++){
            lhs[i] = lhs[i-1] * nums[i-1];
        }
        for(int i = lhs.size()-1; i >= 1 ; i--){
            rhs *= nums[i];
            lhs[i-1] *= rhs;
        }
        return lhs;
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 24 MB