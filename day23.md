# DAY23

## 1、修剪二叉搜索树

#### [669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root==nullptr) return root;
        if(root->val<low){//小于low的剪掉左子树
            TreeNode* right=trimBST(root->right,low,high);
            return right;//返回当前节点的右子树
        }    
        if(root->val>high){//大于high的剪掉右子树
            TreeNode* left=trimBST(root->left,low,high);
            return left;//返回当前节点的左子树
        }
        //如果root->left指向的节点满足两个if条件之一，那么root->left接收到的节点是root->left->left或者root->left->right,相当于去掉了原来的root->left
        //root->right同理
        root->left=trimBST(root->left,low,high);
        root->right=trimBST(root->right,low,high);
        return root;
    }
};
```



## 2、将有序数组转换为二叉搜索树

#### [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

有序数组的中间值就是中间节点，不断切割左右子数组找中间节点即可

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
    TreeNode* traversal(vector<int>& nums,int left,int right){
        if(left>right) return nullptr;
        int mid=left+(right-left)/2;//中间值
        TreeNode* cur=new TreeNode(nums[mid]);//以中间值为值创建新的节点
        cur->left=traversal(nums,left,mid-1);//左区间
        cur->right=traversal(nums,mid+1,right);//右区间
        return cur;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        TreeNode* root=traversal(nums,0,nums.size()-1);
        return root;
    }
};
```



## 3、把二叉搜索树转换为累加树

#### [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

定义全局变量sum记录节点累加值

按照右中左的顺序遍历二叉搜索树

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
    int sum=0;
    void traversal(TreeNode* node){
        if(node==nullptr) return ;
        traversal(node->right);
        sum += node->val;
        node->val=sum;
        traversal(node->left);
        return ;

    }
public:
    TreeNode* convertBST(TreeNode* root) {
        traversal(root);
        return root;
    }
};
```

