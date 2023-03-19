# DAY18

## 1、找树左下角的值

#### [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

迭代法，层序遍历，每到新的一层就记录第一个元素，直到遍历至最后一层

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
    int findBottomLeftValue(TreeNode* root) {
        int result=root->val;
        if(root->left==nullptr&&root->right==nullptr) return result;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            result=que.front()->val;
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                que.pop();
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
        }
        return result;
    }
};
```

递归法，主要是寻找深度更深的节点

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
    int maxDepth=INT_MIN;
    int result;
    void getleft(TreeNode* node,int depth){
        if(node->left==nullptr&&node->right==nullptr){//终止条件
            if(depth>maxDepth){//深度更大时才会更新结果
                maxDepth=depth;
                result=node->val;
            }
        }
        if(node->left){//左
            getleft(node->left,depth+1);
        }
        if(node->right){//右
            getleft(node->right,depth+1);
        }
    }
    int findBottomLeftValue(TreeNode* root) {
        getleft(root,1);
        return result;
    }
};
```



## 2、路径总和

#### [112. 路径总和](https://leetcode.cn/problems/path-sum/)



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
    bool getSum(TreeNode* node,int& targetSum,int sum){
        if(node==NULL) return false;
        if(node->left==NULL&&node->right==NULL&&sum==targetSum) return true;
        if(node->left){
            if(getSum(node->left,targetSum,sum+node->left->val)) return true;
        }
        if(node->right){
            if(getSum(node->right,targetSum,sum+node->right->val)) return true;
        }
        return false;
    }
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root==NULL) return false;
        bool result=getSum(root,targetSum,root->val);
        return result;
    }
};
```

#### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)



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
    vector<vector<int>> result;//全局变量result
    void getpath(TreeNode* node,int targetSum,int sum,vector<int> path){
        if(node==NULL) return ;

        if(node->left==NULL&&node->right==NULL&&sum==targetSum){//终止条件
            result.push_back(path);
            return ;
        }
        if(node->left){//左
            path.push_back(node->left->val);
            getpath(node->left,targetSum,sum+node->left->val,path);//迭代
            path.pop_back();//回溯
        }
        if(node->right){//右
            path.push_back(node->right->val);
            getpath(node->right,targetSum,sum+node->right->val,path);
            path.pop_back();
        }

    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<int> path;
        result.clear();
        if(root==NULL) return result;
        path.push_back(root->val);
        getpath(root,targetSum,root->val,path);
        return result;
    }
};
```



## 3、从中序与后序遍历序列构造二叉树

#### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

后序遍历序列的最后一个元素就是当前的中间节点

以这个中间节点切割中序序列，则可以得到左子树序列和右子树序列，然后对后序序列进行切割

再对左子树序列和右子树序列进行上述过程，直到到达叶子节点

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
    TreeNode* traversal (vector<int>& inorder, vector<int>& postorder) {
        if (postorder.size() == 0) return NULL;
        
        // 后序为左右中
        // 后序遍历数组最后一个元素，就是当前的中间节点
        int rootValue = postorder[postorder.size() - 1];
        TreeNode* root = new TreeNode(rootValue);

        // 只有一个节点，则表示该节点为叶子节点
        if (postorder.size() == 1) return root;

        // 找到中序遍历的切割点
        int delimiterIndex;
        for (delimiterIndex = 0; delimiterIndex < inorder.size(); delimiterIndex++) {
            if (inorder[delimiterIndex] == rootValue) break;
        }

        // 切割中序数组
        // 左闭右开区间：[0, delimiterIndex)
        vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
        // [delimiterIndex + 1, end)
        vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end() );

        // postorder序列里删除最后一个元素，然后再进行切割
        postorder.resize(postorder.size() - 1);

        // 切割后序数组
        // 依然左闭右开，注意这里使用了左中序数组大小作为切割点
        // [0, leftInorder.size)
        vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
        // [leftInorder.size(), end)
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());

        root->left = traversal(leftInorder, leftPostorder);
        root->right = traversal(rightInorder, rightPostorder);

        return root;
    }
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) return NULL;
        return traversal(inorder, postorder);
    }
};

```

