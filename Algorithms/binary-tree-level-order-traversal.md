## 102. Binary Tree Level Order Traversal

### Descirption 
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

#### Constraints
- The number of nodes in the tree is in the range [0, 2000].
- -1000 <= Node.val <= 1000

### Sol 1: using the breadth first search

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> answer;
        if(!root) return answer;  //if root is NULL then return
        queue<TreeNode*> q;  //for storing nodes
        q.push(root);  //push root initially to the queue
        while(!q.empty())  //while queue is not empty go and follow few steps
        {
            int size=q.size();  //storing queue size for while loop
            vector<int> v;  //for storing nodes at the same level
            while(size--)  
            {
                TreeNode* temp=q.front();  //store front node of queue  and pop it from queue
                q.pop();
                v.push_back(temp->val);  //push it to v
                if(temp->left) q.push(temp->left);  //if left subtree exist for temp then store it into the queue
                if(temp->right) q.push(temp->right);  //if right subtree exist for temp then store it into the queue
            }
            answer.push_back(v);  //push v into answer, as v consist of all the nodes of current level
        }
        return answer;  //return the answer
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 12.7 MB
### Sol 2: using depth first search
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
    vector<vector<int>> answer;
    void DFS_levelorder(TreeNode* root,int level) 
    {
        if(!root) return;  //if root is NULL then return
        if(level==answer.size())  //if level is equal to answer size then we have to push one empty element into it
            answer.push_back(vector<int> ());  //pushing extra element to accomodate next level nodes
        answer[level].push_back(root->val);  //pushing current node to the level index of answer
        DFS_levelorder(root->left,level+1);  //recursive call to traverse left subtree by increasing the level order
        DFS_levelorder(root->right,level+1);  //recursive call to traverse right subtree by increasing the level order
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        DFS_levelorder(root,0);  //calling function to traverse whole tree by passing root and level order
        return answer;  
    }
};
```
Note:
- Runtime: 4 ms
- Memory Usage: 13.5 MB

