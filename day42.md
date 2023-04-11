# DAY42

## 1、分割等和子集

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum += nums[i];
        }
        if(sum % 2 == 1) return false;
        vector<int> dp(sum/2+1,0);
        for(int i=0;i<nums.size();i++){
            for(int j=sum/2;j>=nums[i];j--){
                dp[j]=max(dp[j],dp[j-nums[i]]+nums[i]);
            }
        }
        if(dp[sum/2]==sum/2) return true;
        else return false;
    }
};
```



