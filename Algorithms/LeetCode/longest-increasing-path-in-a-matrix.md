## 329. Longest Increasing Path in a Matrix

### Descirption 
Given an m x n integers matrix, return the length of the longest increasing path in matrix.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

#### Constraints
- m == matrix.length
- n = matrix[i].length
- 1 <= m, n <= 200
- 0 <= matrix[i][j] <= 231 - 1

### Sol 1: DFS 

```C++
class Solution {
private:
    void dfs(vector<vector<int>>& matrix, int i, int j, int& n, int& m, vector<vector<int>>& depth) {
        // int n = matrix.size(), m = matrix[0].size();
        depth[i][j] = 1;
        int tmp = depth[i][j];
        if(i + 1 < n)
        {
            if(matrix[i][j] < matrix[i+1][j] )
            {
                if(depth[i+1][j] == 0)
                    dfs(matrix, i+1, j, n, m, depth);
                tmp = max(tmp, depth[i][j] + depth[i+1][j]);
            }
                
        }
        if(i - 1 >= 0)
        {
            if(matrix[i][j] < matrix[i-1][j] )
            {
                if(depth[i-1][j] == 0)
                    dfs(matrix, i-1, j, n, m, depth);
                tmp = max(tmp, depth[i][j] + depth[i-1][j]);
            }
                
        }
        if(j + 1 < m)
        {
            if(matrix[i][j] < matrix[i][j+1] )
            {
                if(depth[i][j+1] == 0)
                    dfs(matrix, i, j+1, n, m, depth);
                tmp = max(tmp, depth[i][j] + depth[i][j+1]);
            }
                
        }
        if(j - 1 >= 0)
        {
            if(matrix[i][j] < matrix[i][j-1])
            {
                if(depth[i][j-1] == 0)
                    dfs(matrix, i, j-1, n, m, depth);
                tmp = max(tmp, depth[i][j] + depth[i][j-1]);
            }
            
        }
        depth[i][j] = tmp;        
    }
    
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        vector<vector<int>> depth(matrix.size(), vector<int> (matrix[0].size(), 0));
        int ret = 1;
        int n = matrix.size(), m = matrix[0].size();
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                if(depth[i][j] == 0)
                    dfs(matrix, i, j, n, m, depth);
                // cout << i << ", " << j << ": " << depth[i][j] << endl;
                if(ret < depth[i][j])
                    ret = depth[i][j];
            }
        }
        return ret;
    }
};
```
Note:
- Runtime: 61 ms
- Memory Usage: 15.8 MB