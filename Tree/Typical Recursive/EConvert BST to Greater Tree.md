## [538. Convert BST to Greater Tree](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

**Example:**

```
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13

Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
```

## Global variable 

* we  iterate the tree according to right - root - left , and we maintain a global variable to record the `sum` until the current node 

```c++
class Solution {
public:
    int num = 0;
    TreeNode* convertBST(TreeNode* root) {
        if(root){
            convertBST(root->right);
            root->val+=num;
            num = root->val;
            convertBST(root->left);
        }
        return root ;
    }
};
```

## vector to record all values

```c++
class Solution {
public:
    int index = 0;
    TreeNode* convertBST(TreeNode* root) {
        if(!root) return root;
        vector<int> tree;
        dfs(root,tree);
        for(int i = tree.size()-2;i>=0;i--){
             tree[i] +=tree[i+1];
        }
        index = 0;
        assign(root,tree);
        return root;
    }
    void dfs(TreeNode * root,vector<int> & tree){
        if(root){
            dfs(root->left,tree);
            tree.push_back(root->val);
            dfs(root->right,tree);
        }
    }
    void assign(TreeNode * root,vector<int> & tree){
        if(root){
            assign(root->left,tree);
            root->val = tree[index++];
            assign(root->right,tree);
        }
    }
};
```

