## 200. Number of Islands

### Descirption 
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

#### Constraints
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 50
- grid[i][j] is either '0' or '1'.


### Sol 1: BFS 

```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        
        int dst[][2] {-1, 0, 1, 0, 0, -1, 0, 1};
        
        // int area = 0, areaN = 0;
        int cnt = 0;
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == '1')
                {
                    // obtain area of current island
                    // areaN = 1;
                    queue<pair<int, int>> path;
                    path.push({i, j});
                    grid[i][j] = '0'; 
                    cnt++;
                    while(!path.empty())
                    {
                        int size = path.size();
                        while(size-- > 0)
                        {
                            pair<int,int> id = path.front();
                            int idi = id.first, idj = id.second;
                            for(int k = 0; k < 4; k++)
                            {
                                int idii = idi + dst[k][0];
                                int idji = idj + dst[k][1];
                                if(idii >= 0 && idii < m && idji >= 0 && idji < n)
                                {
                                    if(grid[idii][idji] == '1')
                                    {
                                        grid[idii][idji] = '0';
                                        path.push({idii, idji});
                                        // areaN++;
                                    }
                                }
                            }
                            path.pop();
                        }
                    }
                    
                    // area = max(area, areaN);
                }
            }
        }
        return cnt;
    }
};
```
Note:
- Runtime: 59 ms
- Memory Usage: 18.2 MB

### Sol 2: DFS
```C++
class Solution {
    void dfs(vector<vector<char>>& grid, int i, int j, int &count) {
        int n = grid.size(), m = grid[0].size();
        if(i < 0 || j < 0 || i >= n || j >= m || grid[i][j] == '0')
            return;        
        grid[i][j]='0';
        count++;
        dfs(grid,i,j+1,count);
        dfs(grid,i,j-1,count);
        dfs(grid,i+1,j,count); 
        dfs(grid,i-1,j,count);    
    }
public:   
    int numIslands(vector<vector<char>>& grid) {        
        int n = grid.size(), m = grid[0].size(), ans = 0, count = 0; 
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++)
                if(grid[i][j] == '1') 
                { 
                    dfs(grid, i, j, count);
                    if(count > 0)
                        ans++;
                    count = 0; 
                }
        return ans;        
    }
};
```
Note:
- Runtime: 49 ms
- Memory Usage: 12.4 MB

**Optimized**
```C++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int cc = 0;
        for(int i = 0; i < grid.size(); i++){
            for(int j = 0; j < grid[i].size(); j++){
                if(grid[i][j] == '1') cc++, dfs(i,j,grid);
            }
        }
        return cc;
    }
    
    void dfs(int i, int j, vector<vector<char>>& grid){
        grid[i][j] = '.';
        if(i+1 < grid.size() && grid[i+1][j] == '1') dfs(i+1,j,grid);
        if(j+1 < grid[i].size() && grid[i][j+1] == '1') dfs(i,j+1,grid);
        if(i-1 >= 0 && grid[i-1][j] == '1') dfs(i-1,j,grid);
        if(j-1 >= 0 && grid[i][j-1] == '1') dfs(i,j-1,grid);
    }
};
```
Note:
- Runtime: 38 ms
- Memory Usage: 12.2 MB
