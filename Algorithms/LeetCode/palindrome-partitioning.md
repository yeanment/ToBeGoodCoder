## 131. Palindrome Partitioning

### Descirption 
Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

A palindrome string is a string that reads the same backward as forward.

#### Constraints
- 1 <= s.length <= 16
- s contains only lowercase English letters.

### Sol 1: DFS, backtracking
```C++
class Solution {
private:
    void dfs(string& s, int i, vector<string>& path, map<int, vector<int>>& palindrome, vector<vector<string>>& ret)
    {
        if(i == s.size())
            ret.push_back(path);
        for(int ii = 0; ii < palindrome[i].size(); ii++)
        {
            int id = palindrome[i][ii];
            path.push_back(s.substr(i, id - i + 1));
            dfs(s, id+1, path, palindrome, ret);
            path.pop_back();
        }
    }

public:
    vector<vector<string>> partition(string s) {
        map<int, vector<int>> palindrome;
        // Get palindrome subsequences
        for(int i = 0; i < s.size()*2; i++)
        {
            int il = i/2, ir = (i + 1)/2;
            while(il >= 0 && ir < s.size())
            {
                if(s[il] == s[ir])
                {
                    palindrome[il].push_back(ir);
                    il--; ir++;
                }
                else
                    break;
            }
        }
        // for(map<int, vector<int>>::iterator it = palindrome.begin(); it != palindrome.end(); it++)
        // {
        //     sort(it->second.begin(), it->second.end());
        // }

        vector<string> path;
        vector<vector<string>> ret;
        dfs(s, 0, path, palindrome, ret);

        return ret;
    }
};
```
Note:
- Runtime: 107 ms
- Memory Usage: 49.3 MB

**Another approach without map**
```C++
class Solution {
public:
    //INTUITION: We can go to each index and process all the valid palindromic substrings after that till the end of string.
    //ALGO: Recursion + Backtracking
    bool isPalindrome(string& s, int i, int j)
    {
        while(i<=j)
        {
            if(s[i]!=s[j]) return false;
            ++i,--j;
        }
        return true;
    }
    void util(int i, vector<string>&curr, vector<vector<string>>&ans, string& s)
    {
        if(i==s.size()) {ans.emplace_back(curr); return;}
        
        for(int t = i; t<s.size(); ++t)
            if(isPalindrome(s,i,t))
            {curr.emplace_back(s.substr(i,t-i+1)); util(t+1,curr,ans,s); curr.pop_back();}
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>> ans;
        vector<string> curr;
        util(0,curr,ans,s);
        return ans;
    }
};
```
Note:
- Runtime: 112 ms
- Memory Usage: 49.1 MB