## 1071. Greatest Common Divisor of Strings

### Descirption 
For two strings s and t, we say "t divides s" if and only if s = t + ... + t  (t concatenated with itself 1 or more times)

Given two strings str1 and str2, return the largest string x such that x divides both str1 and str2.

#### Constraints
- 1 <= str1.length <= 1000
- 1 <= str2.length <= 1000
- str1 and str2 consist of English uppercase letters.

### Sol 1:

```C++
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if(str1.length() == str2.length())
        {
            if(str1 == str2)
                return str1;
        }
        else if(str1.length() > str2.length())
        {
            if(str1.substr(0, str2.length())  == str2)
                return gcdOfStrings(str1.substr(str2.length(), str1.length()), str2);
        }
        else if(str1.length() < str2.length())
        {
            if(str2.substr(0, str1.length())  == str1)
                return gcdOfStrings(str2.substr(str1.length(), str2.length()), str1);
        }
        return "";
    }
};
```
Note:
- Runtime: 8 ms
- Memory Usage: 7.7 MB

**Further optimized**
```C++
class Solution {
    // there is gcd method available in <numeric> starting 
    // c++17 standard or not official __gcd function
    // but we want to have our own implementation
    int gcd(int a, int b) {
        return b ? gcd(b, a % b) : a;
    }
public:
    string gcdOfStrings(string str1, string str2) {
        return (str1 + str2 == str2 + str1) ? str1.substr(0, gcd(str1.size(), str2.size())) : "";
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7.2 MB

