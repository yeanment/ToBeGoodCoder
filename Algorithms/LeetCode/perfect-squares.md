## 279. Perfect Squares

### Descirption 
Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

#### Constraints
- 0 <= n <= 10,000

### Sol 1: DFS
```C++
class Solution {
private:
    unordered_map<int, int> mapSave;
public:
    int numSquares(int n) {
        int minNum = n;
        if(n == 0)
            return 0;
        if(mapSave.find(n) != mapSave.end())
            return mapSave[n];
        for(int i = (int)sqrt(n); i > 1; i--)
        {
            if(n > i*i*minNum)
                break;
            int cntNow = numSquares(n - i*i) + 1;
            if(cntNow < minNum)
                minNum = cntNow;
        }
        mapSave[n] = minNum; 
        return minNum;
    }
};
```
Note:
- Runtime: 99 ms
- Memory Usage: 13.4 MB

```C++
class Solution {
public:
    int numSquares(int n) {
        if(n<0)return 0;
        static vector<int> hg({0});
        while(hg.size()<=n){
            int temp=INT_MAX;
            int m=hg.size();
            for(int i=1;i*i<=m;i++){
                temp=min(temp,hg[m-i*i]+1);
            }
            hg.push_back(temp);
        }
        return hg[n];
    }
};```
Note:
- Runtime: 11 ms
- Memory Usage: 6.4 MB
