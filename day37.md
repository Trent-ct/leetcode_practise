# DAY37

## 1、单调递增的数字

#### [738. 单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

将数字n转化为字符串，就可以很方便地对每一位进行操作（否则需要对n取每一位的数构成一个数组才可以进行操作）

从后向前遍历

前一位大于等于后一位，则后一位置9，前一位--

```c++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string strNum=to_string(n);
        int flag=strNum.size();
        for(int i=strNum.size()-1;i>0;i--){
            if(strNum[i-1]>strNum[i]){
                flag=i;
                strNum[i-1]--;
            }
        }
        for(int i=strNum.size()-1;i>=flag;i--){
            strNum[i]='9';
        }
        return stoi(strNum);
    }
};
```



## 2、监控二叉树

#### [968. 监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

注意空节点的处理，将空节点视作已经被覆盖的节点

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
    int result;
    //0表示无覆盖，1表示有摄像头，2表示有覆盖
    int traversal(TreeNode* cur){
        if(cur == NULL) return 2;
        int left=traversal(cur->left);
        int right=traversal(cur->right);
        //左右都被覆盖了，说明左右均不是摄像头，那么当前节点就还未被覆盖
        if(left == 2 && right == 2) return 0;
        //左节点或右节点有一个没有被覆盖，那么就需要在当前节点添加摄像头
        if(left == 0 || right == 0){
            result++;
            return 1;
        }
        //到这里，左右节点都不为0
        //左节点或右节点有一个有摄像头，那么当前节点就已经被覆盖了
        if(left == 1 || right ==1){
            return 2;
        }

        return -1;

    }
public:
    int minCameraCover(TreeNode* root) {
        result=0;
        //根节点尚未被覆盖的情况
        if(traversal(root) == 0){
            result++;
        }
        return result;
    }
};
```


