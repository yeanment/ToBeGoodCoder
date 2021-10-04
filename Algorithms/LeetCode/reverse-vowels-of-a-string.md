## 345. Reverse Vowels of a String

### Descirption 
Given a string s, reverse only all the vowels in the string and return it.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both cases.

#### Constraints
- 1 <= s.length <= 300,000
- s consist of printable ASCII characters.

### Sol 1:

```C++
class Solution {
public:
    string reverseVowels(string s) {
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'}; 
        std::set<int> first;                           // empty set of ints
        for(int i = 0, j = s.size() - 1; i < j; )
        {
            unordered_set<char>::const_iterator iti = vowels.find(s[i]);
            unordered_set<char>::const_iterator itj = vowels.find(s[j]);
            if(iti == vowels.end())
                i++;
            if(itj == vowels.end())
                j--;
            if(iti != vowels.end() && itj != vowels.end())
            {
                char tmp = s[i];
                s[i] = s[j];
                s[j] = tmp;
                i++; j--;
            }
        }
        return s;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7.9 MB
