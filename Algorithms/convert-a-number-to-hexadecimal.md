## 405. Convert a Number to Hexadecimal

### Descirption 
Given an integer num, return a string representing its hexadecimal representation. For negative integers, two’s complement method is used.

All the letters in the answer string should be lowercase characters, and there should not be any leading zeros in the answer except for the zero itself.

Note: You are not allowed to use any built-in library method to directly solve this problem.

#### Constraints
- -2^31 - 1 <= num <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    string toHex(int num) {
        string ret;
        if(num < 0 )
        {
            num = -(num+1);
            int k = 0;
            while(num != 0)
            {
                int retCode = 15 - (num%16);
                // cout << retCode << endl;
                ret.push_back(retCode > 9 ? 'a' + retCode - 10:'0' + retCode);
                k++;
                num = num/16;
            }
            while(k < 8)
            {
                ret.push_back('f');
                k++;
            }
            for(int i = 0; i < ret.size()/2; i++)
            {
                char tmp = ret[i];
                ret[i] = ret[ret.size()-i-1];
                ret[ret.size() -i-1] = tmp;
            }    
        }
        else if(num == 0)
            ret.push_back('0');
        else
        {
            while(num != 0)
            {
                int retCode =  num%16;
                ret.push_back(retCode > 9 ? 'a' + retCode - 10:'0' + retCode);
                num = num/16;
            }
            // cout << ret << endl;
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
- Runtime: 5 ms
- Memory Usage: 5.9 MB

**Optimized for run time**
```C++
class Solution {
public:
    string toHex(int num) {
        string ret;
        char map[16] = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
        if(num < 0 )
        {
            num = -(num+1);
            int k = 0;
            while(num != 0)
            {
                int retCode = 15 - (num%16);
                // cout << retCode << endl;
                ret.push_back(map[retCode]);
                k++;
                num = num/16;
            }
            while(k < 8)
            {
                ret.push_back('f');
                k++;
            }
        }
        else if(num == 0)
            ret.push_back('0');
        else
        {
            while(num != 0)
            {
                int retCode =  num%16;
                ret.push_back(map[retCode]);
                num = num/16;
            }

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
- Runtime: 0 ms
- Memory Usage: 5.9 MB