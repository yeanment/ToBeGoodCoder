## 1091. Shortest Path in Binary Matrix

### Descirption 
Given an n x n binary matrix grid, return the length of the shortest clear path in the matrix. If there is no clear path, return -1.

A clear path in a binary matrix is a path from the top-left cell (i.e., (0, 0)) to the bottom-right cell (i.e., (n - 1, n - 1)) such that:
- All the visited cells of the path are 0.
- All the adjacent cells of the path are 8-directionally connected (i.e., they are different and they share an edge or a corner).

The length of a clear path is the number of visited cells of this path.

#### Constraints
- n == grid.length
- n == grid[i].length
- 1 <= n <= 100
- grid[i][j] is 0 or 1

### Sol 1: BFS 

```C++
class Solution {
private: 
    void bfs(vector<vector<int>>& grid, int ii, int ij, int& n, vector<vector<int>>& vis)
    {
        vis[ii][ij] = 1;     
        queue<pair<int, int>> q;
        q.push({ii, ij});
        while(!q.empty())
        {
            pair<int, int> id = q.front();
            for(int iii = max(0, id.first - 1); iii < min(id.first + 2, n); iii++)
            {
                for(int iji = max(0, id.second - 1); iji < min(id.second + 2, n); iji++)
                {
                    if((iii != id.first || iji != id.second) && grid[iii][iji] == 0 && vis[iii][iji] == INT_MAX )
                    {
                        vis[iii][iji] = vis[id.first][id.second] + 1;
                        // vis[iii][iji] = min(vis[iii][iji], vis[id.first][id.second] + 1);
                        q.push({iii, iji});
                    }
                }
            }
            q.pop();
        }
    }
public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> vis(n, vector<int> (n, INT_MAX));
        if(grid[0][0] != 0 || grid[n-1][n-1] != 0)
            return -1;
        bfs(grid, n-1, n-1, n, vis);
        return vis[0][0] == INT_MAX? -1: vis[0][0];
    }
};
```
Note:
- Runtime: 70 ms
- Memory Usage: 20.8 MB