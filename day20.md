# DAY20

## 1、最大二叉树

#### [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

每次寻找数组最大值作为中间节点，然后用该中间节点切割数组得到左子树区间和右子树区间

然后进行递归，直到左区间和右区间的大小均为0

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
        if(left>=right) return NULL;
        int maxValueIndex=left;

        for(int i=left;i<right;i++){
            if(nums[i]>nums[maxValueIndex]){
                maxValueIndex=i;
            }
        }
        TreeNode* node=new TreeNode(0);
        node->val=nums[maxValueIndex];

        node->left=traversal(nums,left,maxValueIndex);
        node->right=traversal(nums,maxValueIndex+1,right);
        return node;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return traversal(nums,0,nums.size());
    }
};
```



## 2、合并二叉树

#### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

如果树1为空，则返回树2，此时树2是否为空都不重要

反之，树2为空则返回树1

都不为空对其值进行相加

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
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1==NULL) return root2;
        if(root2==NULL) return root1;
        root1->val+=root2->val;
        root1->left=mergeTrees(root1->left,root2->left);
        root1->right=mergeTrees(root1->right,root2->right);
        return root1;
    }
};
```



## 3、二叉搜索树中的搜索

#### [700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

如果中间节点为空或者值与val相等，则返回中间节点

二叉搜索树中，左子树中的值均小于根节点，右子树中的值均大于根节点

所以根节点值大于val时搜索左子树

根节点值小于val时搜索右子树

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
    TreeNode* searchBST(TreeNode* root, int val) {
        if(root==NULL||root->val==val) return root;
        TreeNode* result=NULL;
        if(root->val>val) result=searchBST(root->left,val);
        if(root->val<val) result=searchBST(root->right,val);
        return result;
    }
};
```



## 4、验证二叉搜索树

#### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

![image-20230320103836487](C:\Users\ct\AppData\Roaming\Typora\typora-user-images\image-20230320103836487.png)

题解中出现了INT的最小值，使用LONG_MIN

中序遍历，按照左中右的顺序不断记录最大值并进行比较，一旦出现当前节点的值小于最大值，则表明不是搜索二叉树

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
//中序遍历
class Solution {
public:
    long long maxVal=LONG_MIN;

    bool isValidBST(TreeNode* root) {
        if(root==NULL) return true;

        bool left=isValidBST(root->left);
        if(root->val>maxVal) maxVal=root->val;
        else return false;
        bool right=isValidBST(root->right);

        return left&&right;
    }
};
```

