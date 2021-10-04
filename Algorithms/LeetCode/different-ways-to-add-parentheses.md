## 241. Different Ways to Add Parentheses

### Descirption 
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

#### Constraints
- 1 <= expression.length <= 20
- expression consists of digits and the operator '+', '-', and '*'.
- All the integer values in the input expression are in the range [0, 99].

### Sol 1:

```C++
class Solution {
public:
    vector<int> diffWaysToCompute(string expression) {
        vector<int> ret;
        set<char> operators = {'+', '-', '*'};
        bool flag = false;
        for(int i = 0; i < expression.size(); i++)
        {
            if(operators.find(expression[i]) != operators.end())
            {
                flag = true;
                vector<int> retL = diffWaysToCompute(expression.substr(0, i));
                vector<int> retR = diffWaysToCompute(expression.substr(i+1, expression.size()));
                for(int j = 0; j < retL.size(); j++)
                {
                    for(int k = 0; k < retR.size(); k++)
                    {
                        switch(expression[i])
                        {
                            case '+': ret.push_back(retL[j]+retR[k]); break;
                            case '-': ret.push_back(retL[j]-retR[k]); break;
                            case '*': ret.push_back(retL[j]*retR[k]); break;
                            default: break;
                        }
                    }
                }
            }
        }
        string::size_type sz;
        if(!flag)
            ret.push_back(stoi(expression, &sz));
        return ret;
    }
};
```
Note:
- Runtime: 22 ms
- Memory Usage: 15.7 MB

**Optimized with memorization**
```C++
class Solution {
public:
    map<string,vector<int>> memo;
    vector<int> diffWaysToCompute(string expression) {
        if(memo.find(expression) != memo.end())     return memo[expression]; 
        vector<int> ret;
        for(int i = 0; i < expression.size(); i++)
        {
            if(expression[i] == '+' || expression[i] == '-' || expression[i] == '*')
            {
                vector<int> retL = diffWaysToCompute(expression.substr(0, i));
                vector<int> retR = diffWaysToCompute(expression.substr(i+1));
                for(int j = 0; j < retL.size(); j++)
                {
                    for(int k = 0; k < retR.size(); k++)
                    {
                        if(expression[i] == '+')
                            ret.push_back(retL[j]+retR[k]); 
                        else if(expression[i] == '-')
                            ret.push_back(retL[j]-retR[k]);
                        else
                            ret.push_back(retL[j]*retR[k]);
                    }
                }
            }
        }
        if(!ret.size())
            ret.push_back(stoi(expression));
        return memo[expression] = ret;
    }
};
```
Note:
- Runtime: 0 ms
- Memory Usage: 7.4 MB
