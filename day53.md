# DAY53

## 1、最长公共子序列

#### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

注意这个子序列可以是不连续的

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1,vector<int>(text2.size()+1,0));
        int result=0;
        for(int i=1;i<=text1.size();i++){
            for(int j=1;j<=text2.size();j++){
                if(text1[i-1]==text2[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                if(dp[i][j]>result) result=dp[i][j];
            }
        }
        return result;
    }
};
```



## 2、不相交的线

#### [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/)

核心与上一题一致，就是求最长公共子序列（不连续但不能改变顺序）

```c++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size()+1,vector<int>(nums2.size()+1,0));
        int result=0;
        for(int i=1;i<=nums1.size();i++){
            for(int j=1;j<=nums2.size();j++){
                if(nums1[i-1]==nums2[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                if(dp[i][j]>result) result=dp[i][j];
            }
        }
        return result;
    }
};
```



## 3、最大子数组和

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

该题的动态规划解

当前的状态可以由前一状态加上当前值获得

或者是由当前值开始重新计算

取两者较大的一个

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size(),0);
        dp[0]=nums[0];
        int result=dp[0];
        for(int i=1;i<nums.size();i++){
            dp[i]=max(dp[i-1]+nums[i],nums[i]);
            if(dp[i]>result) result=dp[i];
        }
        return result;
    }
};
```

