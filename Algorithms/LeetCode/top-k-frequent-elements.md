## 347. Top K Frequent Elements

### Descirption 
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

#### Constraints
- 1 <= nums.length <= 100,000
- k is in the range [1, the number of unique elements in the array].
- It is guaranteed that the answer is unique.

### Sol 1: Binary Search

```C++
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 1)
            return x;
        int l = 1, h = x;
        while(l <= h)
        {
            int m = l + (h - l)/2;
            double sqr = x/m;
            if((int)sqr == m)
                return m;
            if(sqr < m)
                h = m - 1;
            else
                l = m + 1;            
        }
        return h;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.9 MB
