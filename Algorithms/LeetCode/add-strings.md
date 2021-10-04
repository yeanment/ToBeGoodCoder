## 415. Add Strings

### Descirption 
Given two non-negative integers, num1 and num2 represented as string, return the sum of num1 and num2 as a string.

You must solve the problem without using any built-in library for handling large integers (such as BigInteger). You must also not convert the inputs to integers directly.

#### Constraints
- 1 <= num1.length, num2.length <= 10,000
- num1 and num2 consist of only digits.
- num1 and num2 don't have any leading zeros except for the zero itself.

### Sol 1:

```C++
class Solution {
public:
    string addStrings(string num1, string num2) {
        int i = num1.size() - 1, j = num2.size() - 1, carry = 0;
        string ret;
        while (carry > 0 || i >= 0 || j >= 0) {
            if (i >= 0) {
                carry += (num1[i--] - '0');
            }
            if (j >= 0) {
                carry += (num2[j--] - '0');
            }
            // cout << carry << endl;
            ret += (carry % 10 + '0');
            carry /= 10;
        }
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 6.8 MB
