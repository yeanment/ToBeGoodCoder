## 257. Binary Tree Paths

### Descirption 
Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

#### Constraints
- The number of nodes in the tree is in the range [1, 100].
- -100 <= Node.val <= 100

### Sol 1: DFS, Backtracking

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
private:
    void dfs(TreeNode* tree, string pathN, vector<string>& result)
    {
        if(tree == NULL)
            return;
        if(tree->left == NULL && tree->right == NULL)
            result.push_back(pathN + to_string(tree->val));
        dfs(tree->left, pathN + to_string(tree->val) + "->", result);
        dfs(tree->right, pathN + to_string(tree->val) + "->", result);
    }
    
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> ret;
        dfs(root, "", ret);
        return ret;
    }
};
```
Note:
- Runtime: 3 ms
- Memory Usage: 13 MB
