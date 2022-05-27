## 37. Sudoku Solver

### Descirption 
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:
1. Each of the digits 1-9 must occur exactly once in each row.
2. Each of the digits 1-9 must occur exactly once in each column.
3. Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

The '.' character indicates empty cells.

#### Constraints
- board.length == 9
- board[i].length == 9
- board[i][j] is a digit or '.'.
- It is guaranteed that the input board has only one solution.



### Sol 1: DFS, backtracking

```C++
class Solution {
private:
    bool dfs(vector<vector<char>>& board, int id, vector<vector<bool>>& rows, vector<vector<bool>>& cols, vector<vector<bool>>& boxes)
    {
        // check valid?
        if(id == 81)
        {
            return true;
        }
        for(int idi = id; idi < 81; idi ++)
        {
            int i = idi/9, j = idi%9;
            int boxid = int(i/3) * 3 + (int)(j/3);
            // recursive for all possible value of i, j
            // cout << i << ", " << j << ", " << boxid << ": " << endl;
            if(board[i][j] == '.')
            {
                
                for(int ii = 0; ii < 9; ii++)
                {
                    if(rows[i][ii] == false && cols[j][ii] == false && boxes[boxid][ii] == false)
                    {
                        // cout << i << ", " << j << ", " << boxid << ": " << ii << endl;
                        board[i][j] = '1' + ii;
                        rows[i][ii] = true;
                        cols[j][ii] = true;
                        boxes[boxid][ii] = true;
                        if(dfs(board, idi + 1, rows, cols, boxes))
                            return true;
                        board[i][j] = '.';
                        rows[i][ii] = false;
                        cols[j][ii] = false;
                        boxes[boxid][ii] = false;
                    }
                }
                return false;
            }
        }
        return true;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        int n = 9;
        if(board.size() != n || board[0].size() != n)
            return;
        // Initialize for three groups, trick for seek possible sol
        vector<vector<bool>> rows(9, vector<bool> (9, false));
        vector<vector<bool>> cols(9, vector<bool> (9, false));
        vector<vector<bool>> boxes(9, vector<bool> (9, false));

        for(int i = 0; i < 9; i++)
        {
            for(int j = 0; j < 9; j++)
            {
                if(board[i][j] != '.')
                {
                    rows[i][board[i][j] - '1'] = true;
                    cols[j][board[i][j] - '1'] = true;
                    int boxid = int(i/3) * 3 + (int)(j/3);
                    boxes[boxid][board[i][j] - '1'] = true;
                }
            }
        }

        dfs(board, 0, rows, cols, boxes);
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 6.3 MB
