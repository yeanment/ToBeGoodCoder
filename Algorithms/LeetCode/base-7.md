## 504. Base 7

### Descirption 
Given an integer num, return a string of its base 7 representation.

#### Constraints
- -10,000,000 <= num <= 10,000,000

### Sol 1:

```C++
class Solution {
public:
    string convertToBase7(int num) {
        string ret;
        if(num < 0 )
        {
            ret.push_back('-');
            ret.append(convertToBase7(-num));
        }
        else if(num == 0)
            ret.append("0");
        else
        {
            while(num != 0)
            {
                ret.push_back('0' + num%7);
                num = num/7;
            }
            for(int i = 0; i < ret.size()/2; i++)
            {
                char tmp = ret[i];
                ret[i] = ret[ret.size()-i-1];
                ret[ret.size() -i-1] = tmp;
            }

        }        
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.9 MB
