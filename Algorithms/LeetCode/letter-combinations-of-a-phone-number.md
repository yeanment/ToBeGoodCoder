## 17. Letter Combinations of a Phone Number

### Descirption 
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

#### Constraints
- 0 <= digits.length <= 4
- digits[i] is a digit in the range ['2', '9'].

### Sol 1: DFS

```C++
class Solution {
private:
    const vector<vector<char>> map{{'a', 'b', 'c'}, {'d', 'e', 'f'}, {'g', 'h', 'i'}, {'j', 'k', 'l'}, {'m', 'n', 'o'}, {'p', 'q', 'r', 's'}, {'t', 'u', 'v'}, {'w', 'x', 'y', 'z'}};

private:
    vector<string> dfs(string& digits, int i)
    {
        vector<string> ret;
        if(i < digits.size())
        {
            int id = digits[i] - '2';
            for(int j = 0; j < map[id].size(); j++)
            {
                vector<string> ret0 = dfs(digits, i + 1);
                if(ret0.size() == 0)
                {
                    string cur(1, map[id][j]);
                    ret.push_back(cur);
                }
                else
                {
                    for(int k = 0; k < ret0.size(); k++)
                    {
                        string cur(1, map[id][j]);
                        ret.push_back(cur + ret0[k]);
                    }
                }
            }
        }
        return ret;
    }
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ret;
        return dfs(digits, 0);
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6.8 MB

### Sol 2: Backtracking
```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty()) {
            return {};
        }
        
        vector<string> words;
        const auto &abc = vector<string>{"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        function<void(const string&, uint8_t)> backtrack;
        backtrack = [&](const string& s, uint8_t d) {
            if (s.length() == digits.length()) {
                words.emplace_back(s);
                return;
            }
            
            for (auto c : abc[digits[d] - '2']) {
                backtrack(s + c, d + 1);
            }
        };
        
        backtrack("", 0);
        return words;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6.6 MB