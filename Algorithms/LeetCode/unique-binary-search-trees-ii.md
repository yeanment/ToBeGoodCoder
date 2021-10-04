## 95. Unique Binary Search Trees II

### Descirption 
Given an integer n, return all the structurally unique BST's (binary search trees), which has exactly n nodes of unique values from 1 to n. Return the answer in any order.

#### Constraints
- 1 <= n <= 8

### Sol 1:

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    map<pair<int, int>,vector<TreeNode*>> memo;
    vector<TreeNode*> generateTrees(pair<int, int> input) {
        if(memo.find(input) != memo.end()) 
            return memo[input]; 
        int iL = input.first, iR = input.second;
        vector<TreeNode*> ret;
        if(iL > iR) return ret;
        if(iL == iR)
        {
            TreeNode* onTree = new TreeNode(iL);
            ret = {onTree};
        }
        else
        {
            pair<int, int> inputL(iL+1, iR);
            vector<TreeNode*> retL = generateTrees(inputL);
            for(int j = 0; j < retL.size(); j++)
            {
                TreeNode* onTreeL = new TreeNode(iL, nullptr, retL[j]);
                ret.push_back(onTreeL);
            }
            
            pair<int, int> inputR(iL, iR-1);
            vector<TreeNode*> retR = generateTrees(inputR);
            for(int k = 0; k < retR.size(); k++)
            {
                TreeNode* onTreeR = new TreeNode(iR, retR[k], nullptr);
                ret.push_back(onTreeR);
            }
            
            for(int i = iL + 1; i <= iR - 1; i ++)
            {
                inputL = make_pair(iL, i - 1);
                inputR = make_pair(i + 1, iR);
                retL = generateTrees(inputL);
                retR = generateTrees(inputR);
                for(int j = 0; j < retL.size(); j++)
                {
                    for(int k = 0; k < retR.size(); k++)
                    {
                        TreeNode* onTree = new TreeNode(i, retL[j], retR[k]);
                        ret.push_back(onTree);
                    }
                }

            }
        }
        return memo[input] = ret;
    }
    
    vector<TreeNode*> generateTrees(int n) {
        pair<int, int> input(1, n);
        return generateTrees(input);
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 13.4 MB
