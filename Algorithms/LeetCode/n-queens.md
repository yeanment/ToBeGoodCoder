## 51. N-Queens

### Descirption 
The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

#### Constraints
- 1 <= n <= 9

### Sol 1: DFS, backtracking

```C++

class Solution {
private:
    void dfs(int n, int id, vector<string>& path, vector<bool>& cols, vector<bool>& rc1, vector<bool>& rc2, vector<vector<string>>& ret)
    {
        if(id == n)
        {
            ret.push_back(path);
        }
        for(int i = id; i < n; i ++)
        {
            for(int j = 0; j < n; j++)
            {
                if(path[i][j] == '.')
                {
                    // check is possible to put a 
                    if(cols[j] == false && rc1[i + j] == false && rc2[j - i + n] == false)
                    {
                        path[i][j] = 'Q';
                        cols[j] = true; 
                        rc1[i + j] = true;
                        rc2[j - i + n] = true;
                        dfs(n, id + 1, path, cols, rc1, rc2, ret);
                        path[i][j] = '.';
                        cols[j] = false; 
                        rc1[i + j] = false;
                        rc2[j - i + n] = false;
                    }
                }
            }
            return;
        }

    }

public:
    vector<vector<string>> solveNQueens(int n) {
        // DFS
        vector<string> intial(n, string (n, '.'));
        // checked board
        vector<bool> rows(n, false);
        vector<bool> cols(n, false);
        vector<bool> rc1(n*2, false);
        vector<bool> rc2(n*2, false);
        vector<vector<string>> ret;
        dfs(n, 0, intial, cols, rc1, rc2, ret);
        return ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 7.1 MB
