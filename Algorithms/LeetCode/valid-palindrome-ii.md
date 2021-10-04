## 680. Valid Palindrome II

### Descirption 
Given a string s, return true if the s can be palindrome after deleting at most one character from it.

#### Constraints
- 1 <= s.length <= 100,000
- s consists of lowercase English letters.

### Sol 1:

```C++
class Solution {
public:
    bool validPalindrome(string s) {
        string ret = s;
        int iL, jR;
        bool flag = false;
        for(int i = 0, j = s.size() - 1; i < j; i++, j--)
        {
            if(ret[i] != ret[j])
            {
                iL = i; jR = j; flag = true;
                break;
            }
        }
        if(flag)
        {
            for(int i = iL+1, j = jR; i < j; i++, j--)
            {
                if(ret[i] != ret[j])
                {
                    flag = false;
                    break;
                }
            }
            if(!flag)
            {
                for(int i = iL, j = jR-1; i < j; i++, j--)
                {
                    if(ret[i] != ret[j])
                    {
                        return false;
                    }
                }
            }

        }
        return true;
    }
};
```
Note:
- Runtime: 76 ms
- Memory Usage: 22.9 MB

**Optimized for concise code**
```C++
class Solution {
public:
    bool validPalindrome(string s) {
        return validPal(s, 0, s.size() - 1);
    }

    bool validPal(string &s, int i, int j, bool usedSkip = false) {
        for(; i < j; i++, j--)
            if(s[i] != s[j]) 
                return usedSkip ? false : validPal(s, i + 1, j, true) || validPal(s, i, j - 1, true);
        return true;        
    }
};
```
Note:
- Runtime: 64 ms
- Memory Usage: 19.5 MB
