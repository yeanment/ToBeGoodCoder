## 32. Longest Valid Parentheses

### Descirption 
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

#### Constraints
- 0 <= s.length <= 3 * 104
- s[i] is '(', or ')'.

### Sol 1: Stack & greedy
```C++
class Solution {
public:
    int longestValidParentheses(string s) {   
        stack<int> path;
        vector<int> pos(s.length(), 0);
        for(int i = 0; i < s.length(); i++)
        {
            if(s[i] == '(')
                path.push(i);
            else if(s[i] == ')')
            {
                if(!path.empty())
                {
                    pos[i] = path.top();
                    pos[pos[i]] = i;
                    path.pop();
                }
                else
                    pos[i] = -1;
            }
        }
        while(!path.empty())
        {
            pos[path.top()] = -1;
            path.pop();
        }

        int ret = 0, cntLen = 0;
        for(int i = 0; i < s.length(); )
        {
            if(pos[i] != -1)
            {
                cntLen += (pos[i] - i + 1);
                i = pos[i] + 1;
            }
            else
            {
                ret = max(ret, cntLen);
                cntLen = 0;
                i++;
            }
        }
        ret = max(ret, cntLen);
        return ret;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7.9 MB

**Optimized for one path approach**
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        
	    int maxVal = 0;
    	stack<int> st;
	    st.push(-1);
    	for(int i = 0; i < s.size(); i++)
	    	if(s[i] == '(')
                st.push(i);
    		else {       
	    		st.pop();
		    	if(st.empty())
                    st.push(i);
    			else
                    maxVal = max(maxVal, i - st.top());
    		}        
        
	    return maxVal;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7.3 MB

**Optimized for space, two path**
```C++
class Solution {
public:
    int longestValidParentheses(string s) {
        int open=0,close=0, res=0;
        // forward traverse:
        for(auto ch:s){
            if(ch=='(') open++;
            else close++;
            if(open==close) res=max(res,open+close);
            else if(close>open) open=close=0;
        }
        open=close=0;
        // backward traverse:
        for(int i=s.size()-1;i>=0;i--){
            if(s[i]=='(') open++;
            else close++;
            if(open==close) res=max(res,open+close);
            else if(open>close) open=close=0;
        }
        return res;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 6.7 MB