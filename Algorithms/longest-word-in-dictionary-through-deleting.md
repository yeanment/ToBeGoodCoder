## 524. Longest Word in Dictionary through Deleting

### Descirption 
Given a string s and a string array dictionary, return the longest string in the dictionary that can be formed by deleting some of the given string characters. If there is more than one possible result, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

#### Constraints
- 1 <= s.length <= 1000
- 1 <= dictionary.length <= 1000
- 1 <= dictionary[i].length <= 1000
- s and dictionary[i] consist of lowercase English letters.

### Sol 1:

```C++
class Solution {
public:
    static bool cmpString(string s1, string s2)
    {
        if(s1.size() > s2.size())
            return true;
        else if(s1.size() == s2.size())
            return s1 < s2? true:false;
        return false;
    }
    static bool isPossible(string s, string s1)
    {
        if(s1.size() > s.size())
            return false;
        for(int i = 0, j = 0; i < s1.size(); i++)
        {
            j = s.find(s1[i], j);
            if(j == string::npos)
                return false;
            j++;
        }
        return true;
    }
    string findLongestWord(string s, vector<string>& dictionary) {
        sort(dictionary.begin(), dictionary.end(), cmpString);
        // for(int i = 0; i < dictionary.size(); i++)
        //     cout << dictionary[i] << endl;
        for(int i = 0; i < dictionary.size(); i++)
        {
            if(isPossible(s, dictionary[i]))
                return dictionary[i];
        }
        return "";
    }
};
```
Note:
- Runtime: 84 ms
- Memory Usage: 84.5 MB

**Optimized without sorting**
```C++
class Solution {
public:
    static bool isPossible(string s, string s1)
    {
        if(s1.size() > s.size())
            return false;
        for(int i = 0, j = 0; i < s1.size(); i++)
        {
            j = s.find(s1[i], j);
            if(j == string::npos)
                return false;
            j++;
        }
        return true;
    }
    string findLongestWord(string s, vector<string>& dictionary) {
        string ret="";
        for(int i = 0; i < dictionary.size(); i++)
        {
            if(dictionary[i].size() > s.size())
                continue;
            else
            {
                if(isPossible(s, dictionary[i]))
                {
                    if(dictionary[i].size()>ret.size())
                        ret = dictionary[i];
                    else if(dictionary[i].size()==ret.size())
                    {
                        if(dictionary[i]<ret)
                            ret = dictionary[i];
                    }
                } 
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 48 ms
- Memory Usage: 22.7 MB
