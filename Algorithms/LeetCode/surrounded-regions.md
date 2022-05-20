## 130. Surrounded Regions

### Descirption 
Given an m x n matrix board containing 'X' and 'O', capture all regions that are 4-directionally surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

#### Constraints
- m == board.length
- n == board[i].length
- 1 <= m, n <= 200
- board[i][j] is 'X' or 'O'.



### Sol 1: DFS 

```C++
class Solution {
private:
    void dfs(vector<vector<char>>& board, int i, int j, int& m, int& n)
    {
        
        if(i < 0 || i >= m || j < 0 || j >= n)
            return;
        if(board[i][j] == 'O')
        {
            board[i][j] = 'P';
            dfs(board, i+1, j, m, n);
            dfs(board, i-1, j, m, n);
            dfs(board, i, j+1, m, n);
            dfs(board, i, j-1, m, n);
        }
    }

    
public:
    void solve(vector<vector<char>>& board) {
        int m = board.size(), n = board[0].size();
        if(m < 3 || n < 3)
            return;
        // BFS or DFS
        for(int i = 0; i < m; i++)
        {
            if(board[i][0] == 'O')
                dfs(board, i, 0, m, n);
            if(board[i][n-1] == 'O')
                dfs(board, i, n-1, m, n);
        }
        for(int i = 1; i < n - 1; i++)
        {
            if(board[0][i] == 'O')
            {
                dfs(board, 0, i, m, n);
            }
            if(board[m-1][i] == 'O')
            {
                dfs(board, m-1, i, m, n);
            }
        }
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
                if(board[i][j] == 'P')
                    board[i][j] = 'O';
            }
        }
    }
};
```
Note:
- Runtime: 35 ms
- Memory Usage: 10 MB

### Sol 2: BFS
```C++
class Solution
{
public:
    vector<pair<int, int>> dir = {{0, 0}, {1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    void solve(vector<vector<char>> &board)
    {
        int n = board.size(), m = board[0].size();
        queue<pair<int, int>> q;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if ((i == 0 || j == 0 || i == n - 1 || j == m - 1) && board[i][j] == 'O')
                {
                    q.push({i, j});
                }
            }
        }

        while (!q.empty())
        {
            auto [x, y] = q.front();
            q.pop();

            for (auto i : dir)
            {
                int X = x + i.first;
                int Y = y + i.second;
                if (X >= 0 && X < n && Y >= 0 && Y < m && board[X][Y] == 'O')
                {
                    board[X][Y] = 'Y';
                    q.push({X, Y});
                }
            }
        }

        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (board[i][j] == 'Y')
                    board[i][j] = 'O';
                else
                    board[i][j] = 'X';
            }
        }
    }
};
```
Note:
- Runtime: 19 ms
- Memory Usage: 10.1 MB