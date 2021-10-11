## 392. Is Subsequence

### Descirption 
Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

#### Constraints
- 0 <= s.length <= 100
- 0 <= t.length <= 10,000
- s and t consist only of lowercase English letters.

### Sol 1: 

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int ids = 0, idt = 0;
        while(ids < s.size() && idt < t.size())
        {
            if(t[idt++] == s[ids])
                ids++;
        }
        return ids == s.size();
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6.3 MB
