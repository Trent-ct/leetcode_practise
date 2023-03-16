# DAY15

## 1、二叉树的层序遍历

#### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

一层一层按顺序遍历二叉树

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if(root==nullptr) return result;//二叉树为空时，直接返回结果
        que.push(root);
        while(!que.empty()){//只要队列不为空，则表明还有下一层子节点
            int size=que.size();
            vector<int> vec;
            for(int i=0;i<size;i++){//遍历每一层，将根节点的子节点插入到队列中
                TreeNode* cur=que.front();
                vec.push_back(cur->val);
                que.pop();
                if(cur->left!=nullptr) que.push(cur->left);
                if(cur->right!=nullptr) que.push(cur->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```



## 2、二叉树的层序遍历 II

#### [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)



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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> result;
        queue<TreeNode*> que;
        if(root==nullptr) return result;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            vector<int> vec;
            for(int i=0;i<size;i++){//遍历每一层，将根节点的子节点插入到队列中
                TreeNode* cur=que.front();
                que.pop();
                vec.push_back(cur->val);
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
            result.push_back(vec);
        }
        reverse(result.begin(),result.end());//在正序层级遍历的基础上加一个逆序
        return result;
    }
};
```



## 3、二叉树的右视图

#### [199. 二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)



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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if(root==nullptr) return result;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            TreeNode* cur;
            for(int i=0;i<size-1;i++){
                cur=que.front();
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
                que.pop();
            }
            cur=que.front();
            result.push_back(cur->val);
            if(cur->left) que.push(cur->left);
            if(cur->right) que.push(cur->right);
            que.pop();
        }
        return result;
    }
};
```



## 4、二叉树的层平均值

#### [637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

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
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> que;
        vector<double> result;
        if(root==nullptr) return result;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            double sum=0;
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                sum+=cur->val;
                que.pop();
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
            result.push_back(sum/size);
        }
        return result;
    }
};
```

## 5、N 叉树的层序遍历

#### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> result;
        if(root==nullptr) return result;
        queue<Node*> que;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            vector<int> vec;
            for(int i=0;i<size;i++){
                Node* cur=que.front();
                que.pop();
                vec.push_back(cur->val);
                for(int j=0;j<cur->children.size();j++){
                    if(cur->children[j]!=nullptr)
                    que.push(cur->children[j]);
                }
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

## 6、在每个树行中找最大值

#### [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

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
    vector<int> largestValues(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> que;
        if(root==nullptr) return result;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            int max=que.front()->val;
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                que.pop();
                if(cur->val>max) max=cur->val;
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
            }
            result.push_back(max);
        }
        return result;
    }
};
```

## 7、填充每个节点的下一个右侧节点指针

#### [116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
      if(root==NULL) return root;
      queue<Node*> que;
      que.push(root);
      while(!que.empty()){
          int size=que.size();
          for(int i=0;i<size;i++){
              Node* cur=que.front();
              que.pop();
              if(i<size-1) cur->next=que.front();
              if(cur->left) que.push(cur->left);
              if(cur->right) que.push(cur->right);
          }
        } 
        return root; 
    }
};
```

## 8、填充每个节点的下一个右侧节点指针 II

#### [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

与上一题一样

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
      if(root==NULL) return root;
      queue<Node*> que;
      que.push(root);
      while(!que.empty()){
          int size=que.size();
          for(int i=0;i<size;i++){
              Node* cur=que.front();
              que.pop();
              if(i<size-1) cur->next=que.front();
              if(cur->left) que.push(cur->left);
              if(cur->right) que.push(cur->right);
          }
       } 
        return root;  
    }
};
```

## 9、二叉树的最大深度

#### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

层序遍历，层数等于深度

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
    int maxDepth(TreeNode* root) {
        int depth=0;
        queue<TreeNode*> que;
        if(root==nullptr) return depth;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            depth++;
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                que.pop();
                if(cur->left!=nullptr) que.push(cur->left);
                if(cur->right!=nullptr) que.push(cur->right);
            }
        }
        return depth;
    }
};
```

## 10、二叉树的最小深度

#### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

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
    int minDepth(TreeNode* root) {
        int depth=0;
        if(root==nullptr) return depth;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int size=que.size();
            depth++;
            for(int i=0;i<size;i++){
                TreeNode* cur=que.front();
                que.pop();
                if(cur->left) que.push(cur->left);
                if(cur->right) que.push(cur->right);
                if(!cur->right&&!cur->left) return depth; 
            }
        }
        return depth;
    }
};
```

## 11、翻转二叉树

#### [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

不断将将父节点的两个左右子节点交换顺序即可

注意递归结束条件

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
    TreeNode* invertTree(TreeNode* root) {
        if(root==nullptr) return root;
        swap(root->left,root->right);//交换父节点的左右子节点
        invertTree(root->left);//交换左子节点的左右子节点
        invertTree(root->right);//交换右子节点的左右子节点
        return root;
    }
};
```

## 12、对称二叉树

#### [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

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
    bool compare(TreeNode* left,TreeNode* right){
        if(left==nullptr&&right!=nullptr) return false;//一个为空另一个不为空则不相等
        else if(left!=nullptr&&right==nullptr) return false;
        else if(left==nullptr&&right==nullptr) return true;//两个均为空表示相等
        else if(left->val!=right->val) return false;//两个值不相等则返回false
        
        //当值相等时，进行下一层的比较
        bool outside=compare(left->left,right->right);//为判断是否对称，外侧与外侧比较
        bool inside=compare(left->right,right->left);//内侧与内侧比较
        bool issame=outside&&inside;//当内外侧均相等时，返回true
        return issame;
    }
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr) return true;
        return compare(root->left,root->right);
        //从底层一层一层回溯到一开始均为true时，则为对称二叉树
        //否则，有一个false出现，则表明不对称
    }
};
```

