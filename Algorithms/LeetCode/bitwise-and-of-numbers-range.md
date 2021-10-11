## 201. Bitwise AND of Numbers Range

### Descirption 
Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.

#### Constraints
- 0 <= left <= right <= 2^31 - 1

### Sol 1: 
```C++
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        return (n > m) ? (rangeBitwiseAnd(m/2, n/2) << 1) : m;
    }
};

class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int trans = 0;
        while(m != n) 
        {
            ++ trans;
            m >>= 1;
            n >>= 1;
        }
        return m << trans;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 5.9 MB
