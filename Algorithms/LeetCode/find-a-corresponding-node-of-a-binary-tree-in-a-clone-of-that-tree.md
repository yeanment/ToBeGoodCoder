## 1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree

### Descirption 
Given two binary trees original and cloned and given a reference to a node target in the original tree.

The cloned tree is a copy of the original tree.

Return a reference to the same node in the cloned tree.

Note that you are not allowed to change any of the two trees or the target node and the answer must be a reference to a node in the cloned tree.

#### Constraints
- The number of nodes in the tree is in the range [1, 104].
- The values of the nodes of the tree are unique.
- target node is a node from the original tree and is not null.

### Sol 1: 

```C++
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
private:
    TreeNode* findTarget(TreeNode* original, TreeNode* cloned, TreeNode* target){
        // Determine if cloned == target?
        TreeNode* getCloned;
        if(original == NULL)
            return NULL;
        if(original->val == target->val)
            return cloned;
        else 
        {
            getCloned = findTarget(original->left, cloned->left, target);
            if(getCloned == NULL)
            {
                getCloned = findTarget(original->right, cloned->right, target);
                if(getCloned == NULL)
                    return NULL;
            }
            return getCloned;
        }
    }
    
public:
    TreeNode* getTargetCopy(TreeNode* original, TreeNode* cloned, TreeNode* target) {
        // Using recursive programming
        return findTarget(original, cloned, target);
    }
};
```
Note:
- Runtime: 563 ms
- Memory Usage: 163.8 MB
