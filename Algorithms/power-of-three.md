## 326. Power of Three

### Descirption 
Given an integer n, return true if it is a power of three. Otherwise, return false.

An integer n is a power of three, if there exists an integer x such that n == 3^x.

#### Constraints
- -2^31 <= n <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    bool isPowerOfThree(int n) {
        if(n == 0)
            return false;
        while(n != 0)
        {
            if(n % 3 != 0 && n != 1)
                return false;
            n /= 3;
        }
        return true;
    }
};
```
Note:
- Runtime: 8 ms
- Memory Usage: 5.9 MB

### Sol 2:

```C++
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && (1162261467 % n == 0);
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 5.9 MB