# DAY21

## 1、二叉搜索树的最小绝对差

#### [530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)



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
private:
    int min_dif=INT_MAX;
    TreeNode* pre=NULL;
    void traversal(TreeNode* node){
        if(node==NULL) return ;
        traversal(node->left);
        if(pre!=NULL){
            min_dif=min(min_dif,node->val-pre->val);
        }
        pre=node;
        traversal(node->right);

    }
public:
    int getMinimumDifference(TreeNode* root) {
        traversal(root);
        return min_dif;
    }
};
```



## 2、二叉搜索树中的众数

#### [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)



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
private:
    vector<int> result;
    TreeNode* pre=NULL;
    int count=0;
    int maxcount=1;
    void traversal(TreeNode* cur){
        if(cur==NULL) return;
        traversal(cur->left);
        if(pre==NULL){//pre为空时，cur为第一个元素，记录频次为1
            count=1;
        }else if(cur->val==pre->val){//cur的值与pre相等，表示出现相同，频次+1
            count++;
        }else{//pre不为空且与cur的值不想等，表示cur是新的元素，频次重新置为1
            count=1;
        }
        if(count==maxcount){//有频次相同的众数
            result.push_back(cur->val);
        }else if(count>maxcount){//如果出现了频次更高的众数
            maxcount=count;
            result.clear();//之前的众数均无效
            result.push_back(cur->val);
        }
        pre=cur;
        traversal(cur->right);
    }
public:
    vector<int> findMode(TreeNode* root) {
        traversal(root);
        return result;
    }
};
```



## 3、二叉树的最近公共祖先

#### [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)



```c++
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
    TreeNode* traversal(TreeNode* node,TreeNode* p,TreeNode* q){
        if(node==p||node==q||node==NULL) return node;
        TreeNode* left=traversal(node->left,p,q);//左
        TreeNode* right=traversal(node->right,p,q);//右
        //处理中节点时，不能返回node就结束了，回溯过程中，left和right还需要返回值
        if(left!=NULL&&right!=NULL) return node;
        if (left == NULL && right != NULL) return right;
        else if (left != NULL && right == NULL) return left;
        else  { //  (left == NULL && right == NULL)
            return NULL;
        }
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return traversal(root,p,q);   
    }
};
```

