## 93. Restore IP Addresses

### Descirption 
A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.
- For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.

Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

#### Constraints
- 1 <= s.length <= 20
- s consists of digits only.

### Sol 1: DFS 

```C++
class Solution {
private:
    vector<string> dfs(string& s, int i, int cnt)
    {
        vector<string> ret;
        int n = s.length() - i;
        if(n < cnt || n > cnt*3)
            return ret;
        if(cnt == 1)
        {
            string cur(s.substr(i));
            if(cur.length() >= 3 && (stoi(cur) > 255 || stoi(cur) < 100))
                return ret;
            else if(cur.length() == 2 && (stoi(cur) < 10))
                return ret;
            ret.push_back(s.substr(i));
            return ret;
        }
        if(i + 1 < s.length())
        {
            string cur(s.substr(i, 1));
            vector<string> ret0 = dfs(s, i+1, cnt-1);
            if(!ret0.empty())
            {
                for(int j = 0; j < ret0.size(); j++)
                {
                    string in = cur + "." + ret0[j];
                    ret.push_back(cur + "." + ret0[j]);
                }
            }
        }
        if(i + 2 < s.length())
        {
            string cur = s.substr(i, 2);
            if(stoi(cur) > 9)
            {
                vector<string> ret0 = dfs(s, i+2, cnt-1);
                if(!ret0.empty())
                {
                    for(int j = 0; j < ret0.size(); j++)
                    {
                        ret.push_back(cur + "." + ret0[j]);
                    }
                }
            }
        }
        if(i + 3 < s.length())
        {
            string cur = (s.substr(i, 3));
            if(stoi(cur) <= 255 &&  stoi(cur) > 99 )
            {
                vector<string> ret0 = dfs(s, i+3, cnt-1);
                if(!ret0.empty())
                {
                    for(int j = 0; j < ret0.size(); j++)
                    {
                        ret.push_back(cur + "." + ret0[j]);
                    }
                }
            }
        }
        return ret;
    }
    
public:
    vector<string> restoreIpAddresses(string s) {
        int n = s.length();
        vector<string> ret = dfs(s, 0, 4);
        return ret;
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 66.72.1 MB

### Sol 2: DFS
```C++
class Solution {
public:
    // check if a number is valid for ip subpart
    bool isValid(string s) {
        if(s.empty() || (s.size() > 1 && s[0] == '0'))
            return false;
        int num = stoi(s);
        return num >= 0 && num <= 255;
    }
    
    void validIp(int curr, string& s, string ip, 
                 int part_num, vector<string>& result) {
        // base case: when all 4 parts have been made, but there
        // are still digits left. eg: orig: 12112334 => 1.2.1.123 34
        if(part_num == 5 && curr < s.size())
            return;
        // base case: all the numbers covered and all the parts exists i.e 4
        if(curr == s.size() && part_num == 5) {
            result.emplace_back(ip);
            return;
        }
        
        // if there are already existing subparts before
        if(!ip.empty())
            ip += '.';
        
        for(int i = 0; i < 3 && curr + i < s.size(); i++) {
            string ip_part = s.substr(curr, i + 1);
            if(isValid(ip_part))
                validIp(curr+i+1, s, ip + ip_part, part_num+1, result);
        }
    }
    
    vector<string> restoreIpAddresses(string s) {
        vector<string> result;
        validIp(0, s, "", 1, result);
        return result;
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 7 MB