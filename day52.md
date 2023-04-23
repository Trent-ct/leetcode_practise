# DAY52

## 1、最长递增子序列

#### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

每次将nums[i]与nums[j]比较，取众多dp[j]+1中较大的一个

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int result=1;
        vector<int> dp(nums.size(),1);
        for(int i=1;i<nums.size();i++){
            for(int j=0;j<i;j++){
                if(nums[i]>nums[j]) dp[i]=max(dp[i],dp[j]+1);
            }
            if(dp[i]>result) result=dp[i];
        }
        return result;
    }
};
```



## 2、最长连续递增序列

#### [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)

注意是连续的，与上题不同

```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        vector<int> dp(nums.size(),1);
        int result=1;
        for(int i=1;i<nums.size();i++){
            if(nums[i]>nums[i-1]) dp[i]=dp[i-1]+1;
            if(dp[i]>result) result=dp[i];
        }
        return result;
    }
};
```



## 3、最长重复子数组

#### [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)

使用二维数组记录重复比较记录

```c++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size()+1,vector<int>(nums2.size()+1,0));
        int result=0;
        for(int i=1;i<=nums1.size();i++){
            for(int j=1;j<=nums2.size();j++){
                if(nums1[i-1]==nums2[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                if(dp[i][j]>result) result=dp[i][j];
            }
        }
        return result;
    }
};
```

