## 417. Pacific Atlantic Water Flow

### Descirption 
There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

#### Constraints
- m == heights.length
- n == heights[r].length
- 1 <= m, n <= 200
- 0 <= heights[r][c] <= 100,000



### Sol 1: BFS 

```C++
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size(), n = heights[0].size();
        
        vector<vector<int>> ret;

        if(m < 2 || n < 2)
        {
            for(int i = 0; i < m; i++)
            {
                for(int j = 0; j < n; j++)
                {
                    ret.push_back({i,j});
                }
            }
            return ret;
        }
        vector<vector<int>> dst {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        vector<vector<bool>> pacfic(m, vector<bool> (n, 0));
        vector<vector<bool>> atlantic(m, vector<bool> (n, 0));
        // BFS for group 1
        queue<pair<int, int>> path;
        for(int i = 0; i < m; i++)
        {
            path.push({i, 0});
            pacfic[i][0] = true;
        }
        for(int j = 1; j < n; j++)
        {
            path.push({0, j});
            pacfic[0][j] = true;
        }
        while(!path.empty())
        {
            pair<int,int> id = path.front();
            int idi = id.first, idj = id.second;
            for(int k = 0; k < dst.size(); k++)
            {
                int idii = idi + dst[k][0];
                int idji = idj + dst[k][1];
                if(idii >= 0 && idii < m && idji >= 0 && idji < n)
                {
                    if(pacfic[idii][idji]  == false && heights[idii][idji] >= heights[idi][idj])
                    {
                        pacfic[idii][idji] = true;
                        path.push({idii, idji});
                    }
                }
            }
            path.pop();
        }
        
        for(int i = 0; i < m; i++)
        {
            path.push({i, n-1});
            atlantic[i][n-1] = true;
        }
        for(int j = 0; j < n-1; j++)
        {
            path.push({m-1, j});
            atlantic[m-1][j] = true;
        }

        while(!path.empty())
        {
            pair<int,int> id = path.front();
            int idi = id.first, idj = id.second;
            for(int k = 0; k < dst.size(); k++)
            {
                int idii = idi + dst[k][0];
                int idji = idj + dst[k][1];
                if(idii >= 0 && idii < m && idji >= 0 && idji < n)
                {
                    if(atlantic[idii][idji] == false && heights[idii][idji] >= heights[idi][idj])
                    {
                        atlantic[idii][idji] = true;
                        path.push({idii, idji});
                    }
                }
            }
            path.pop();
        }
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(pacfic[i][j] && atlantic[i][j])
                    ret.push_back({i,j});
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 57 ms
- Memory Usage: 18.1 MB

### Sol 2: DFS
```C++
class Solution {
    vector<array<int, 2>> directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    const char unknown = ' ';
    
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights.front().size();
        
        vector<vector<char>> region(m, vector<char>(n, unknown));
        
        vector<vector<int>> result;
        function<void(int, int, char)> dfs = [&](int i, int j, char ocean) {
            if (region[i][j] == ocean) return;
			
			// check if the entry is also in another ocean region
            if (region[i][j] != unknown) result.push_back({i, j});
            
            region[i][j] = ocean;
            
            for (auto [di, dj] : directions) {
                int ii = i + di;
                int jj = j + dj;
                if (ii < 0 || ii >= m || jj < 0 || jj >= n) continue;
                if (heights[ii][jj] < heights[i][j]) continue;
                
                dfs(ii, jj, ocean);
            }
        };
        
		// search for pacific region
        for (int j = 0; j < n; ++j) dfs(0, j, 'p');
        for (int i = 0; i < m; ++i) dfs(i, 0, 'p');
		
		// search for atlantic region
        for (int j = 0; j < n; ++j) dfs(m - 1, j, 'a');
        for (int i = 0; i < m; ++i) dfs(i, n - 1, 'a');
        
        return result;
    }
};
```
Note:
- Runtime: 52 ms
- Memory Usage: 17.5 MB