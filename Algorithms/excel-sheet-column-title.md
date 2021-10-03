## 168. Excel Sheet Column Title

### Descirption 
Given an integer columnNumber, return its corresponding column title as it appears in an Excel sheet.

For example:
```html
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```
#### Constraints
- 1 <= num <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ret;
        char map[26] = {'Z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y'};
        while(columnNumber != 0)
        {
            int retCode =  columnNumber%26;
            ret.push_back(map[retCode]);
            columnNumber = retCode == 0 ? (columnNumber/26 - 1):columnNumber/26;
        }
        for(int i = 0; i < ret.size()/2; i++)
        {
            char tmp = ret[i];
            ret[i] = ret[ret.size()-i-1];
            ret[ret.size() -i-1] = tmp;
        }    
        return ret;
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 5.9 MB

**Optimized for run time**
```C++
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string letter = "ZABCDEFGHIJKLMNOPQRSTUVWXY";
        string res = "";
        while(columnNumber > 0) {
            int rem = columnNumber % 26;
            res += letter[rem];
            columnNumber = columnNumber/26;
            if(rem == 0)
                columnNumber -= 1;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6 MB