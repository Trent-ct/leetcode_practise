# DAY16

## 1、二叉树的最大深度

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)  递归法

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
//后序左右中，先处理左右子树，然后处理中间节点
class Solution {
public:
    int getDepth(TreeNode* root){
        if(root==nullptr) return 0;

        int leftdepth=getDepth(root->left);//左子树
        int rightdepth=getDepth(root->right);//右子树
        int maxdepth=1+max(leftdepth,rightdepth);//处理中间节点
        return maxdepth;
    }
    int maxDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```



## 2、二叉树的最小深度

#### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)  递归法

注意分支为空并不代表是最小深度，最小深度表示到叶子节点的最短路径对应的深度

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
    int getDepth(TreeNode* root){
        if(root==nullptr) return 0;

        int leftdepth=getDepth(root->left);//左子树深度
        int rightdepth=getDepth(root->right);//右子树深度

        if(root->left==nullptr&&root->right!=nullptr)
            return 1+rightdepth;//左子树为空，返回右子树深度
        if(root->right==nullptr&&root->left!=nullptr)
            return 1+leftdepth;//右子树为空

        return 1+min(leftdepth,rightdepth);//其余情况，取左右子树的最小深度
    }
    int minDepth(TreeNode* root) {
        return getDepth(root);
    }
};
```



## 3、完全二叉树的节点个数

#### [222. 完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

迭代法，使用层序遍历，每弹出一个节点，节点数++

弹出一个节点时，将节点的左右子节点压入队列中

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
    int countNodes(TreeNode* root) {
        int num=0;
        if(root==nullptr) return num;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                que.pop();
                num++;
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
        }
        return num;
    }
};
```

递归法

先求左右子树的节点个数，左右子树节点个数和再加上中间节点就是总节点数

终止条件为节点为空

左右中后序递归

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
    int getNodeNum(TreeNode* root){
        if(root==nullptr) return 0;
        int leftnum=getNodeNum(root->left);
        int rightnum=getNodeNum(root->right);
        return 1+leftnum+rightnum;
    }
    int countNodes(TreeNode* root) {
        return getNodeNum(root);
    }
};
```

使用满二叉树节点个数计算公式，2^(最大深度)-1

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
    int countNodes(TreeNode* root) {
        if(root==nullptr) return 0;
        TreeNode* left=root->left;
        TreeNode* right=root->right;
        int left_depth=1;//初始有中间节点，深度为1
        int right_depth=1;
        while(left){//左子树深度
            left=left->left;
            left_depth++;
        }
        while(right){//右子树深度
            right=right->right;
            right_depth++;
        }
        if(left_depth==right_depth){//左右深度相同，表示为满二叉树
            return (2<<left_depth-1)-1;
        }
        return countNodes(root->left)+countNodes(root->right)+1;
    }
};
```

