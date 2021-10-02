## 204. Count Primes

### Descirption 
Given an integer n, return the number of prime numbers that are strictly less than n.

#### Constraints
- 0 <= n <= 5,000,000

### Sol 1:

```C++
class Solution {
public:
    int countPrimes(int n) {
        int count = 0;
        vector<bool> notPrimes(n+1, false);
        for(int i = 2; i < n; i++)
        {
            if(notPrimes[i])
                continue;
            count++;
            for(long j = (long)(i)*i; j < n; j+=i)
                notPrimes[(int)j] = true;
        }
        return count;
    }
};
```
Note:
- Runtime: 216 ms
- Memory Usage: 10.4 MB