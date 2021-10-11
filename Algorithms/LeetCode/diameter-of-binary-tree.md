## 543. Diameter of Binary Tree

### Descirption 
Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

#### Constraints
- The number of nodes in the tree is in the range [1, 10,000].
- -100 <= Node.val <= 100

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
    int longnestPathToRootOfBinaryTree(TreeNode* root) {
        if(root == nullptr)
            return 0;
        return max(longnestPathToRootOfBinaryTree(root->left), longnestPathToRootOfBinaryTree(root->right)) + 1;      
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if(root == nullptr)
            return 0;
        int ret = max(diameterOfBinaryTree(root->left), diameterOfBinaryTree(root->right));
        return max(longnestPathToRootOfBinaryTree(root->left) + longnestPathToRootOfBinaryTree(root->right), ret);
    }
};
```
Note:
- Runtime: 16 ms
- Memory Usage: 20.2 MB
**Optimzed in reduce the recursive time**
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

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        // Check if tree exists
        if (root == NULL) return 0;
        
        // Find depths of left and right subtrees
        int leftDepth = DFS(root->left);
        int rightDepth = DFS(root->right);
        
        // Return largest diameter (maybe not through root)
        return max(diam, leftDepth + rightDepth);
    }
    
// Good practice to make internal methods and class variables private
private:
    
    // Class variable to save max diameter found
    int diam = 0;
    
    // Recursive DFS method
    int DFS(TreeNode* root) {
        // Check for leaf node
        if (root == NULL) return 0;
        
        // Find depths of left and right subtrees
        int leftDepth = DFS(root->left);
        int rightDepth = DFS(root->right);
        
        // Diameter is the number of edges between the farthest nodes
        diam = max(diam, leftDepth + rightDepth);
        
        // Return the depth of the current subtree
        return max(leftDepth, rightDepth) + 1;
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 20.3MB
