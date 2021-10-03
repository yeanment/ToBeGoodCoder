## 633. Sum of Square Numbers

### Descirption 
Given a non-negative integer c, decide whether there're two integers a and b such that a^2 + b^2 = c.

#### Constraints
- 0 <= c <= 2^31 - 1

### Sol 1:

```C++
class Solution {
public:
    bool judgeSquareSum(int c) {
        for(int i = 0, j = (int)sqrt(c), sum = i*i + j*j; i <= j; )
        {
            if(sum == c)
                return true;
            else if(sum > c)
            {
                sum -= (2*j-1);
                j--;
            }
            else 
            {
                int ret = c - (2*i + 1);
                if(sum == ret)
                    return true;
                else if( sum > ret)
                {
                    sum -= (2*j-1);
                    sum += (2*i+1);
                    i++; j--;
                    
                }
                else
                {
                    sum += (2*i+1);
                    i++;                    
                }
                
            }
        }
        return false;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.9 MB
