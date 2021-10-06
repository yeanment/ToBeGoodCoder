## 278. First Bad Version

### Descirption 
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

#### Constraints
- 1 <= bad <= n <= 2^31 - 1

### Sol 1: Binary Search

```C++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        if(isBadVersion(1)) 
            return 1;
        int l = 1, h = n;
        while(l < h)
        {
            int mid = l + (h - l)/2;
            if(mid == l)
                break;
            if(isBadVersion(mid))
            {
                h = mid;
            }
            else
            {
                l = mid;
            }

        }
        return h;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.8 MB
