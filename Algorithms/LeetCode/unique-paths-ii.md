## 63. Unique Paths II

### Descirption 
You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m-1][n-1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2,000,000,000.


#### Constraints
- m == obstacleGrid.length
- n == obstacleGrid[i].length
- 1 <= m, n <= 100
- obstacleGrid[i][j] is 0 or 1.

### Sol 1: Brute force DFS (TLE)

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        // DFS search
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        int ret = 0;
        if(obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1)
            return ret;
        if(m == 1 && n == 1)
            return ++ret;
        stack<pair<int,int>> path;
        stack<int> opt;
        path.push({0, 0});
        opt.push(0);
        while(!path.empty())
        {
            pair<int, int> idN = path.top();
            int optN = opt.top();
            int idi = idN.first, idj = idN.second;
            if(optN == 0)
            {
                opt.pop();
                opt.push(++optN);
                int idii = idi + 1;
                if(idii == m - 1 && idj == n - 1)
                {
                    ret++; continue;
                }
                if(idii < m && obstacleGrid[idii][idj] == 0)
                {
                    path.push({idii, idj});
                    opt.push(0);
                }
            }
            else if(optN == 1)
            {
                opt.pop();
                opt.push(++optN);
                int idji = idj + 1;
                if(idi == m - 1 && idji == n - 1)
                {
                    ret++; continue;
                }
                if(idji < n && obstacleGrid[idi][idji] == 0)
                {
                    path.push({idi, idji});
                    opt.push(0);
                }
            }
            else
            {
                opt.pop();
                path.pop();
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: TLE
- Memory Usage: 

### Sol 2: Dynamic programming
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        // Dynamic programming
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        int ret = 0;
        if(obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1)
            return ret;
        if(m == 1 && n == 1)
            return ++ret;
        vector<vector<long long int>> path(m, vector<long long int> (n, 0));
        path[m-1][n-1] = 1;
        
        for(int i = m -1; i >= 0; i--)
        {
            for(int j = n-1; j >= 0; j--)
            {
                if(obstacleGrid[i][j] == 0)
                {
                    if(i + 1 < m) //  && obstacleGrid[i+1][j] == 0) 
                    {
                        path[i][j] += path[i + 1][j];
                    }
                    if(j + 1 < n) // && obstacleGrid[i][j+1] == 0)
                    {
                        path[i][j] += path[i][j + 1];
                    }
                }
            }
        }
        ret = path[0][0];
        return ret;
    }
};
```
Note:
- Runtime: 5 ms
- Memory Usage: 7.8 MB