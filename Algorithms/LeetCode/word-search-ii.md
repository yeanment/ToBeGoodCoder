## 212. Word Search II

### Descirption 
Given an m x n board of characters and a list of strings words, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

#### Constraints
- m == board.length
- n == board[i].length
- 1 <= m, n <= 12
- board[i][j] is a lowercase English letter.
- 1 <= words.length <= 3 * 104
- 1 <= words[i].length <= 10
- words[i] consists of lowercase English letters.
- All the strings of words are unique.

### Sol 1: DFS Search

```C++
class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> ret;
        m = board.size();
        n = board[0].size();
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                uniqueBoardChar.emplace(board[i][j]);
            }
        }
        for(int i = 0; i < words.size(); i++)
        {
            // vector<vector<char>> board2(board);
            reverse(words[i].begin(), words[i].end());
            if(exist(board, words[i]))
            {
                reverse(words[i].begin(), words[i].end());
                ret.push_back(words[i]);
            }
        }
        return ret;
    }
private:
    int m, n;
    set<char> uniqueBoardChar;

    bool exist(vector<vector<char>>& board, string& word) {
        if (word.empty()) return true;
        if (board.empty() || board[0].empty()) return false;
        
        unordered_map<char, int> freq;
        for(int i = 0; i < word.size(); i++)
        {
            if(uniqueBoardChar.find(word[i]) == uniqueBoardChar.end())
            {
                return false;
            }
        }
        
        
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                // traverse the board to find the first char
                if (dfsSearch(board, word, 0, i, j)) return true;
        return false;
    }
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
- Runtime: 0 ms
- Memory Usage: 9.1 MB

**Optimized with memory**
```C++
class Solution {
public:
    vector<string> res;
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        int m = board.size();
        int n = board[0].size();
        
        for (const auto& word : words) {
            if (table.find(word.back()) == table.end())
                table[word.back()] = {word};
            else
                table[word.back()].insert(word);
        }
        for (int col = 0; col < m; ++col) {
            for (int row = 0; row < n; ++row) {
                if (table.find(board[col][row]) != table.end()) {
                    for (auto word : table[board[col][row]]) {
                        int idx = word.size() - 1;
                        if (finish.find(word) == finish.end() &&
                           dfs(col, row, idx, word, board)) {
                            res.push_back(word);
                            finish.insert(word);
                        }
                    }
                }
            }
        }
        return res;
    }
private:
    unordered_map<char, unordered_set<string> > table;
    unordered_set<string> finish;
    bool dfs(int col, int row, int idx, string& word, vector<vector<char>>& board) {
        if (col < 0 || row < 0 || col >= board.size() || row >= board[col].size() || board[col][row] != word[idx]) {
            return false;
        }
        if (idx == 0) {
            return true;
        }
        char tmp = board[col][row];
        board[col][row] = '*';
        bool res =
            dfs(col+1, row, idx-1, word, board) ||
            dfs(col-1, row, idx-1, word, board) ||
            dfs(col, row+1, idx-1, word, board) ||
            dfs(col, row-1, idx-1, word, board);
        board[col][row] = tmp;
        return res;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 9.3 MB

### Sol 2: With trie
The implementation of trie can be found in [208. Implement Trie (Prefix Tree)](implement-trie-prefix-tree.md)

The trie is used to store the word, and then conduct DFS.

```C++
class TrieNode {
public:
    bool isEnd;
    shared_ptr<TrieNode> next[26];
    TrieNode(): isEnd(false) { }
};

class Trie {
public:
    shared_ptr<TrieNode> root;
public:
    Trie() {
        root = make_shared<TrieNode>();
    }
    Trie(vector<string>& words) {
        root = make_shared<TrieNode>();
        for(int i = 0; i < words.size(); i++)
        {
            insert(words[i]);
        }
    }
    void insert(string& word) {
        shared_ptr<TrieNode> ws = root;
        for(int i = 0; i < word.size(); i++)
        {
            char cur = word[i];
            if(ws->next[cur - 'a'] == nullptr){
                ws->next[cur - 'a'] = std::make_shared<TrieNode>();
            }
            ws = ws->next[cur - 'a'];
        }
        ws->isEnd = true;
    }

    void deleteWord(string& word) {
        deleteWordUtil(word, word.length(), 0, root);
    }
private:
    bool deleteWordUtil(string& word, int n, int depth, shared_ptr<TrieNode> curr) {
        if(depth==n){
            curr->isEnd=false;
            if(isEmpty(curr)){
                return true;
            }
            return false;
        }
        int index=word[depth]-'a';
        if(deleteWordUtil(word,n,depth+1,curr->next[index])){
            curr->next[index]=NULL;
            if(!curr->isEnd && isEmpty(curr)){
                return true;
            }
            return false;
        }
        return false;
    }
    bool isEmpty(shared_ptr<TrieNode> curr) {
        for(int i = 0; i < 26; i++)
            if(curr->next[i])
                return false;
        return true;
    }
};

class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        m = board.size();
        n = (m>0)? board[0].size():0;
        Trie trie(words);
        shared_ptr<TrieNode> root=trie.root;
        vector<string> res;
        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                string word;
                findWord(board, i, j, root, trie, word, res);
            }
        }
        return res;
    }
private:
    int m, n;
    void findWord(vector<vector<char>>& board, int i, int j, shared_ptr<TrieNode> root, Trie& trie, string& word, vector<string>& res) {
        if(i < 0 || i >= m || j < 0 || j >= n || board[i][j]==' ')
            return;
        if(root->next[board[i][j]-'a']){
            word+=board[i][j];
            root=root->next[board[i][j]-'a'];
            if(root->isEnd){
                res.push_back(word);
                trie.deleteWord(word);
            }
            char ch=' ';
            swap(ch,board[i][j]);
            findWord(board, i-1, j, root, trie, word, res);
            findWord(board, i, j+1, root, trie, word, res);
            findWord(board, i+1, j, root, trie, word, res);
            findWord(board, i, j-1, root, trie, word, res);
            swap(ch,board[i][j]);
            word.pop_back();
        }  
    }
};
```
Note:
- Runtime: 40 ms
- Memory Usage: 13.6 MB
