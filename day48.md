# DAY48

## 1、打家劫舍

#### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

对当前家，小偷可以选择偷或者不偷，那么当前的价值由dp[i-2]+nums[i]和dp[i-1]较大的决定

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==0) return 0;
        if(nums.size()==1) return nums[0];
        vector<int> dp(nums.size());
        dp[0]=nums[0];
        dp[1]=max(nums[0],nums[1]);
        for(int i=2;i<nums.size();i++){
            dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[nums.size()-1];
    }
};
```



## 2、打家劫舍 II

#### [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)

由于首位相连，所以将首尾分开考虑，如果首偷了，那么尾就一定不能偷，反之亦然

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1) return nums[0];
        //首不偷
        int result1=robRange(nums,1,nums.size()-1);
        //尾不偷
        int result2=robRange(nums,0,nums.size()-2);
        return max(result1,result2);
    }
    int robRange(vector<int>& nums,int left,int right){
        if(left==right) return nums[left];
        vector<int> dp(nums.size());
        dp[left]=nums[left];
        dp[left+1]=max(nums[left],nums[left+1]);
        for(int i=left+2;i<=right;i++){
            dp[i]=max(dp[i-2]+nums[i],dp[i-1]);
        }
        return dp[right];
    }

};
```



## 3、打家劫舍 III

#### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)

注意状态转移数组dp，dp[0]表示不偷当前节点的最大价值，dp[1]表示偷当前节点的最大价值

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
    int rob(TreeNode* root) {
        vector<int> result=robTree(root);
        return max(result[0],result[1]);
    }
    vector<int> robTree(TreeNode* cur){
        if(cur==nullptr) return vector<int>{0,0};
        vector<int> left=robTree(cur->left);
        vector<int> right=robTree(cur->right);
        //当前节点不偷，可以偷左右孩子或者不偷左右孩子
        int val1=max(left[0],left[1])+max(right[0],right[1]);
        //偷当前节点，左右孩子都不可偷
        int val2=cur->val+left[0]+right[0];
        return {val1,val2};
    }
    
};
```

