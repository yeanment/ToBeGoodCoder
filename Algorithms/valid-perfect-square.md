## 367. Valid Perfect Square

### Descirption 
Given a **positive** integer num, write a function which returns True if num is a perfect square else False.

**Follow up:** Do not use any built-in library function such as sqrt.

#### Constraints
- 1 <= num <= 2^31 -1 

### Sol 1:

```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        for(int j = 1; j<=num/j;j++)
        {
            if(num == j*j)
                return true;
            // cout << j << endl;
        }
        return false;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.9 MB


### Sol 2:
1=1 \
1+3=4 \
1+3+5=9 \
1+3+5+7=16 \
and so on ..............

```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        int a=1;
        while(num>0){
            num-=a;
            a+=2;
        }
        return(num==0);
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 5.7 MB
