## 647. Palindromic Substrings

### Descirption 
Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

#### Constraints
- 1 <= s.length <= 1000
- s consists of lowercase English letters.

### Sol 1: DFS
```C++
class Solution {
private:
    static bool isPalindromic(string& s, int& i, int& j)
    {
        if(i < 0 || j < i || j > s.length())
            return false;
        for(int ii = i, ij = j - 1; ii < ij; ii++, ij--)
        {
            if(s[ii]!=s[ij])
                return false;
        }
        return true;
    }
private:
    void dfs(string& s, int l, int& cnt)
    {
        if(l > s.length())
        {
            return;
        }
        for(int i = 0, j = i + l; j <= s.length(); i++, j++)
        {
            if(isPalindromic(s, i, j))
            {
                cnt++;
            }
        }
        dfs(s, l+1, cnt);
    }

public:
    int countSubstrings(string s) {
        int cnt = 0;
        dfs(s, 1, cnt);
        return cnt;
    }
};
```
Note:
- Runtime: 314 ms
- Memory Usage: 6.5 MB
### Sol 2: Dynamic Programming
Consider left and right of current pos.
```C++
class Solution {
public:
	int countSubstrings(string s) {
		int ans = 0;

		for(int i = 0; i < s.size(); i++) {
			int left = i, right = i;
			while(left >= 0 && right < s.size() && s[left] == s[right]) {
				ans++;
				left--;
				right++;
			}
			left = i, right = i + 1;
			while(left >= 0 && right < s.size() && s[left] == s[right]) {
				ans++;
				left--;
				right++;
			}
		}

		return ans;
	}
};
```
Note:
- Runtime: 11 ms
- Memory Usage: 6.3 MB

**Optimized**
```C++
class Solution {
public:
    int countSubstrings(string s) {
        int n = s.size();
        int ans = 0;
        for (int i = 0; i < 2 * n - 1; ++i) {
            int l = i / 2;
            int r = l + i % 2;
            while (l >= 0 && r < n 
                   && s[l--] == s[r++])
                ans++;
        }
        return ans;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 6.2 MB

