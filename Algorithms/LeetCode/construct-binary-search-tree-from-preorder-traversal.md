## 1008. Construct Binary Search Tree from Preorder Traversal

### Descirption 
Given an array of integers preorder, which represents the preorder traversal of a BST (i.e., binary search tree), construct the tree and return its root.

It is guaranteed that there is always possible to find a binary search tree with the given requirements for the given test cases.

A binary search tree is a binary tree where for every node, any descendant of Node.left has a value strictly less than Node.val, and any descendant of Node.right has a value strictly greater than Node.val.

A preorder traversal of a binary tree displays the value of the node first, then traverses Node.left, then traverses Node.right.

#### Constraints
- 1 <= preorder.length <= 100
- 1 <= preorder[i] <= 100,000,000
- All the values of preorder are unique.

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
private:
    TreeNode* bstFromPreorder(vector<int>& preorder, int iS, int iE) {
        TreeNode* root;
        if(iS == iE)
            root = nullptr;
        else if(iE - iS == 1)
            root = new TreeNode(preorder[iS]);
        else
        {
            int i;
            root = new TreeNode(preorder[iS]);
            for(i = iS + 1; i < iE; i++)
            {
                if(preorder[i] > preorder[iS])
                {
                    root->right = bstFromPreorder(preorder, i, iE);
                    break;
                }
            }
            root->left = bstFromPreorder(preorder, iS+1, i);
        }
        return root;
    }
public:
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        if(preorder.size() == 0)
            return nullptr;
        return bstFromPreorder(preorder, 0, preorder.size());
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 13.7 MB
