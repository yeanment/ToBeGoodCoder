## 763. Partition Labels

### Descirption 
You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Return a list of integers representing the size of these parts.

#### Constraints
- 0 <= x <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        vector<int> ret;
        vector<int> isGroup(26, -1);
        int gid = 0, iEnd = 1;
        isGroup[s[0] - 'a'] = gid;
        for(int i = 1; i < s.size(); i++)
        {
            // cout << gid << ", " << iEnd << endl;
            if(isGroup[s[i] - 'a'] == -1)
            {
                gid++;
                isGroup[s[i] - 'a'] = gid;
                ret.push_back(iEnd);
                iEnd = 1;
            }
            else if(isGroup[s[i] - 'a'] == gid)
            {
                iEnd++;
            }
            else
            {
                gid = isGroup[s[i] - 'a'];
                for(int j = 0; j < 26; j++)
                {
                    if(isGroup[j] > gid)
                        isGroup[j] = gid;
                }
                for(int j = ret.size() - 1; j >= isGroup[s[i] - 'a']; j--)
                {
                    iEnd += ret.back();
                    ret.pop_back();
                }
                iEnd++;
            }
        }
        ret.push_back(iEnd);
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 6.7 MB
