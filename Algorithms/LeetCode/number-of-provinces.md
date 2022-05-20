## 547. Number of Provinces

### Descirption 
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

#### Constraints
- 1 <= n <= 200
- n == isConnected.length
- n == isConnected[i].length
- isConnected[i][j] is 1 or 0.
- isConnected[i][i] == 1
- isConnected[i][j] == isConnected[j][i]



### Sol 1: BFS 

```C++
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<bool> seen(n, true);
        int cnt = 0;
        for(int i = 0; i < n; i++)
        {
            if(seen[i])
            {
                // BFS or DFS
                seen[i] = false;
                cnt++;
                queue<int> city;
                city.push(i);
                while(!city.empty())
                {
                    int id = city.front();
                    for(int j = 0; j < n; j++)
                    {
                        if(seen[j] && isConnected[id][j])
                        {
                            seen[j] = false;
                            city.push(j);
                        }
                    }
                    city.pop();
                }
            }
        }
        return cnt;
    }
};
```
Note:
- Runtime: 21 ms
- Memory Usage: 14.1 MB

### Sol 2: DFS
```C++
class Solution {
private:
    void dfs(vector<vector<int>>& isConnected, int id, int& n, vector<bool>& seen)
    {
        seen[id] = false;
        for(int i = 0; i < n; i++)
        {
            if(seen[i] && isConnected[id][i])
                dfs(isConnected, i, n, seen);
        }
    }
    
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<bool> seen(n, true);
        int cnt = 0;
        for(int i = 0; i < n; i++)
        {
            if(seen[i])
            {
                cnt++;
                dfs(isConnected, i, n, seen);
            }
        }
        return cnt;
    }
};
```
Note:
- Runtime: 44 ms
- Memory Usage: 13.7 MB