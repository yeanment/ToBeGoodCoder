## 172. Factorial Trailing Zeroes

### Descirption 
Given an integer n, return the number of trailing zeroes in n!.

Note that n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1.

#### Constraints
- 0 <= n <= 10,000

### Sol 1:

```C++
class Solution {
public:
    int trailingZeroes(int n) {
        return n == 0 ? 0 : n / 5 + trailingZeroes(n / 5);
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.9 MB
