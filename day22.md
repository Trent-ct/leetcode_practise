# DAY22

## 1、二叉搜索树的最近公共祖先

#### [235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

若当前节点的值位于p,q组成的左闭右闭区间内，则表明当前节点是p,q的最近公共祖先

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==NULL) return root;
        if(root->val>=min(p->val,q->val)&&root->val<=max(p->val,q->val)){
            return root;
        }
        if(root->val>p->val&&root->val>q->val){
            return lowestCommonAncestor(root->left,p,q);
        }
        if(root->val<p->val&&root->val<q->val){
            return lowestCommonAncestor(root->right,p,q);
        }
        return root;
    }
};
```



## 2、二叉搜索树中的插入操作

#### [701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

二叉搜索树中插入一个元素，一定可以在将其插入到叶子节点中

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
    void traversal(TreeNode* node,TreeNode* target){
        if(target->val<node->val){//目标值小于当前节点值
            if(node->left==nullptr){//且当前节点的左节点为空
                node->left=target;//将目标插入
                return ;    //直接返回
            }
            traversal(node->left,target);//左节点不为空，继续往左递归
        }
        if(target->val>node->val){//目标值大于当前节点值
            if(node->right==nullptr){//右节点为空
                node->right=target;
                return ;  
            }
            traversal(node->right,target);//往右递归
        }
        return ;
    }
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode* target=new TreeNode(val);
        if(root==nullptr){//根节点为空，直接插入
            root=target;
            return root;
        }
        traversal(root,target);
        return root;
    }
};
```



## 3、删除二叉搜索树中的节点

#### [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

将节点值与key做比较，大于key向左遍历，小于key向右遍历

注意删除节点的五种情况

cur指向和root指向相同的一片空间，cur针对空间进行操作，并不影响root

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(root==nullptr) return root;
        if(root->val==key){
            if(root->left!=nullptr&&root->right!=nullptr){
                TreeNode* cur=root->right;//寻找右子树的最左值
                while(cur->left!=nullptr){
                    cur=cur->left;
                }
                cur->left=root->left;//将左子树嫁接到右子树最左值下
                TreeNode* tmp=root;
                root=root->right;//删除当前节点
                delete tmp;
                return root;//返回值被上一节点接住
            }else if(root->left!=nullptr&&root->right==nullptr){//左不为空
                //直接将左子树当作新的树
                TreeNode* tmp=root;
                root=root->left;
                delete tmp;
                return root;
            }else if(root->left==nullptr&&root->right!=nullptr){//右不为空
                //直接将右子树当作新的树返回
                TreeNode* tmp=root;
                root=root->right;
                delete tmp;
                return root;
            }else{//左右都为空，直接删除当前节点即可
                TreeNode* tmp=root;
                root=nullptr;
                delete tmp;
                return root;
            }
        }
        //值大于key，向左遍历
        if(root->val>key) root->left=deleteNode(root->left,key);
        //值小于key，向右遍历
        if(root->val<key) root->right=deleteNode(root->right,key);

        return root;
    }
};
```

