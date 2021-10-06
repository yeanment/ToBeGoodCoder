## 69. Sqrt(x)

### Descirption 
Given a non-negative integer x, compute and return the square root of x.

Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

**Note:** You are not allowed to use any built-in exponent function or operator, such as pow(x, 0.5) or x ** 0.5.

#### Constraints
- 0 <= x <= 2^31 - 1

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
