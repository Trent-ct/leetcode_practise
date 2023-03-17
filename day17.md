# DAY17

## 1、平衡二叉树

#### [110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

```c++
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
    int getheight(TreeNode* node){
        if(node==0) return 0;
        int leftheight=getheight(node->left);//左子树高度
        if(leftheight==-1) return -1;
        int rightheight=getheight(node->right);//右子树高度
        if(rightheight==-1) return -1;
        if(abs(leftheight-rightheight)>1) return -1;//高度差大于1，返回-1，表示不是平衡二叉树
        else return 1+max(leftheight,rightheight);//否则返回中间节点高度

    }
    bool isBalanced(TreeNode* root) {
        if(getheight(root)==-1) return false;
        else return true;
    }
};
```



## 2、二叉树的所有路径

#### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```c++
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
    void getpath(TreeNode* root,string path,vector<string>& result){//形参path，并不会对实际path的值进行修改
        //中节点
        path+=to_string(root->val);//将当前节点加入到路径中
        if(root->left==nullptr&&root->right==nullptr){//左右子节点均为空，表示该节点为叶子节点
            result.push_back(path);
            return ;
        }
        //左节点
        if(root->left) getpath(root->left,path+"->",result);
        //右节点
        if(root->right) getpath(root->right,path+"->",result);
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        string path;
        getpath(root,path,result);
        return result;
    }
};
```



## 3、左叶子之和

#### [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

递归法，采用中左右的前序遍历方式

```c++
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
    void getLeftLeaves(TreeNode* node,int& sum){
        if(node==nullptr) return ;//当前节点为空，那么不会再有叶子
        if(node->left==nullptr&&node->right==nullptr) return ;//节点的左右子节点为空，该节点为叶子

        if(node->left!=nullptr&&node->left->left==nullptr&&node->left->right==nullptr){
            sum+=node->left->val;
        }

        if(node->left) getLeftLeaves(node->left,sum);
        if(node->right) getLeftLeaves(node->right,sum);


    }
    int sumOfLeftLeaves(TreeNode* root) {
        int result=0;
        if(root->left==nullptr&&root->right==nullptr) return result;
        getLeftLeaves(root,result);
        return result;
    }
};
```

采用左右中的后序遍历方式

```c++
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
    int sumOfLeftLeaves(TreeNode* root) {
        if(root==nullptr) return 0;//不存在叶子
        if(root->left==nullptr&&root->right==nullptr) return 0;//当前节点为叶子
        int leftvalue=sumOfLeftLeaves(root->left);
        if(root->left!=nullptr&&root->left->right==nullptr&&root->left->right==nullptr){
            leftvalue=root->left->val;
        }
        int rightvalue=sumOfLeftLeaves(root->right);

        return leftvalue+rightvalue;
        
    }
};
```

