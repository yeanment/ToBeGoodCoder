## 67. Add Binary

### Descirption 
Given two binary strings a and b, return their sum as a binary string.

#### Constraints
- 1 <= a.length, b.length <= 10,000
- a and b consist only of '0' or '1' characters.
- Each string does not contain leading zeros except for the zero itself.

### Sol 1:

```C++
class Solution {
public:
    string addBinary(string a, string b) {
        int i = a.size() - 1, j = b.size() - 1, carry = 0;
        string ret;
        while (carry == 1 || i >= 0 || j >= 0) {
            if (i >= 0 && a[i--] == '1') {
                carry++;
            }
            if (j >= 0 && b[j--] == '1') {
                carry++;
            }
            ret += (carry % 2 + '0');
            carry /= 2;
        }
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 6.3 MB
