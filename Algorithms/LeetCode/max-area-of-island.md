## 695. Max Area of Island

### Descirption 
You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

#### Constraints
- m == grid.length
- n == grid[i].length
- 1 <= m, n <= 50
- grid[i][j] is either 0 or 1.


### Sol 1: BFS 

```C++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        
        int dst[][2] {-1, 0, 1, 0, 0, -1, 0, 1};
        
        int area = 0, areaN = 0;
        int cnt = 2;
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(grid[i][j] == 1)
                {
                    // obtain area of current island
                    areaN = 1;
                    queue<pair<int, int>> path;
                    path.push({i, j});
                    grid[i][j] = cnt++;
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
                                    if(grid[idii][idji] == 1)
                                    {
                                        grid[idii][idji] = cnt;
                                        path.push({idii, idji});
                                        areaN++;
                                    }
                                }
                            }
                            path.pop();
                        }
                    }
                    
                    area = max(area, areaN);
                }
            }
        }
        return area;
    }
};
```
Note:
- Runtime: 26 ms
- Memory Usage: 26.8 MB

### Sol 2: DFS
```C++
class Solution {
    void dfs(vector<vector<int>>& grid, int i, int j, int &count) {
        int n = grid.size(), m = grid[0].size();
        if(i < 0 || j < 0 || i >= n || j >= m || grid[i][j] == 0)
            return;        
        grid[i][j]=0;
        count++;
        dfs(grid,i,j+1,count);
        dfs(grid,i,j-1,count);
        dfs(grid,i+1,j,count); 
        dfs(grid,i-1,j,count);    
    }
public:   
    int maxAreaOfIsland(vector<vector<int>>& grid) {        
        int n = grid.size(), m = grid[0].size(), ans = 0, count = 0; 
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++)
                if(grid[i][j] == 1) { dfs(grid, i, j, count); ans = max(ans, count), count = 0; }
        return ans;        
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 23.1 MB