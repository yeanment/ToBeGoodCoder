## 79. Word Search

### Descirption 
Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

#### Constraints
- m == board.length
- n = board[i].length
- 1 <= m, n <= 6
- 1 <= word.length <= 15
- board and word consists of only lowercase and uppercase English letters.

### Sol 1: BFS 

```C++
class Solution {
public:
    bool exist(const vector<vector<char>>& board, const string word, int iS, int jS)
    {
        int n = board.size(), m = board[0].size();
        vector<vector<int>> visited(n, vector<int>(m, 0));
        visited[iS][jS] = 1;
        
        int idxStr = 1, lenStr = word.size();
        if(lenStr == 1)
            return true;
        stack<vector<int>> stackTo;
        vector<int> onNode = {iS, jS};
        stackTo.push(onNode);
        
        while(!stackTo.empty())
        {
            vector<int> onNodeS = stackTo.top();
            int iN = onNodeS[0], jN = onNodeS[1];
            // cout << "iN = " << iN << ", jN = " << jN << ", visited[iN][jN] = " << visited[iN][jN] << endl;
            // cout << "Working on '" << word[idxStr] << "'" << endl;
            while(visited[iN][jN] < 5)
            {
                // cout << "iN = " << iN << ", jN = " << jN << ", visited[iN][jN] = " << visited[iN][jN] << endl;
                // for(int i = 0; i < n; i++)
                // {
                //     for(int j = 0; j < m; j++)
                //     {
                //         cout<< visited[i][j] << ", " ;
                //     }
                //     cout << endl;
                // }
                // cout << "Working on '" << word[idxStr] << "'" << endl;
                if(visited[iN][jN] == 1)
                {
                    // cout <<  (iN != 0 && visited[iN-1][jN] == 0 ) << endl;
                    if(iN != 0 && visited[iN-1][jN] == 0 && word[idxStr] == board[iN-1][jN])
                    {
                        // cout << " here" << endl;
                        if((++idxStr) == word.size())
                            return true;
                        vector<int> toPush = {iN-1, jN};
                        stackTo.push(toPush);
                        visited[iN-1][jN] = 1;
                        visited[iN][jN]++;
                        break;
                    }
                }
                else if(visited[iN][jN] == 2)
                {
                    if(iN != n - 1  && visited[iN+1][jN] == 0 && word[idxStr] == board[iN+1][jN])
                    {
                        if((++idxStr) == word.size())
                            return true;
                        vector<int> toPush = {iN+1, jN};
                        stackTo.push(toPush);
                        visited[iN+1][jN] = 1;
                        visited[iN][jN]++;
                        break;
                    }
                }
                else if(visited[iN][jN] == 3)
                {
                    if(jN != 0 && visited[iN][jN-1] == 0 && word[idxStr] == board[iN][jN-1])
                    {
                        if((++idxStr) == word.size())
                            return true;
                        vector<int> toPush = {iN, jN-1};
                        stackTo.push(toPush);
                        visited[iN][jN-1] = 1;
                        visited[iN][jN]++;
                        break;
                    }
                }
                else if(visited[iN][jN] == 4)
                {                    
                    if(jN != m - 1 && visited[iN][jN+1] == 0 && word[idxStr] == board[iN][jN+1])
                    {
                        if((++idxStr) == word.size())
                            return true;
                        vector<int> toPush = {iN, jN+1};
                        stackTo.push(toPush);
                        visited[iN][jN+1] = 1;
                        visited[iN][jN]++;
                        break;
                    }
                }
                visited[iN][jN]++;
                
            }
            if(visited[iN][jN] == 5 && stackTo.top()[1] == jN)
            {
                visited[iN][jN] = 0;
                stackTo.pop();
                idxStr--;
            }
        }
        return false;
    }
    
    bool exist(vector<vector<char>>& board, string word) {
        // DFS or BFS search
        // Suppose DFS
        int n = board.size(), m = board[0].size();
        set<int> uniqueWordChar(word.begin(), word.end());
        set<int> uniqueBoardChar(board[0].begin(), board[0].end());
        for(int i = 1; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                // cout << "i = " << i << ", j = " << j << endl;
                uniqueBoardChar.emplace(board[i][j]);
            }
        }
        
        for(set<int>::iterator it = uniqueWordChar.begin(); it != uniqueWordChar.end(); it++)
        {
            if(uniqueBoardChar.find(*it) == uniqueBoardChar.end())
                return false;
        }
        for(int i = 0; i < n; i++)
        {
            for(int j = 0; j < m; j++)
            {
                // cout << "i = " << i << ", j = " << j << endl;
                if(board[i][j] == word[0] && exist(board, word, i, j))
                    return true;
            }
        }
        return false;
    }
};
```
Note:
- Runtime: 880 ms
- Memory Usage: 156.9 MB

```C++
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty()) return true;
        if (board.empty() || board[0].empty()) return false;
        
        m = board.size();
        n = board[0].size();
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                // traverse the board to find the first char
                if (dfsSearch(board, word, 0, i, j)) return true;
        return false;
    }
private:
    int m;
    int n;
    bool dfsSearch(vector<vector<char>>& board, string& word, int k, int i, int j) {
        if (i < 0 || i >= m || j < 0 || j >= n || word[k] != board[i][j]) return false;
        if (k == word.length() - 1) return true;  // found the last char

        char cur = board[i][j];
        board[i][j] = '*';  // used
        bool search_next = dfsSearch(board, word, k+1, i-1 ,j) 
                        || dfsSearch(board, word, k+1, i+1, j) 
                        || dfsSearch(board, word, k+1, i, j-1)
                        || dfsSearch(board, word, k+1, i, j+1);
        board[i][j] = cur;  // reset
        return search_next;
    }
};
```
Note:
- Runtime: 180 ms
- Memory Usage: 7.5 MB
