# DAY43

## 1、最后一块石头的重量 II

#### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)

将一堆石头分为两堆重量相近的石头

```c++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum=0;
        for(int i=0;i<stones.size();i++){
            sum += stones[i];
        }
        vector<int> dp(sum/2 + 1);
        for(int i=0;i<stones.size();i++){
            for(int j=sum/2;j>=stones[i];j--){
                dp[j]=max(dp[j],dp[j-stones[i]]+stones[i]);
            }
        }
        return sum-2*dp[sum/2];
    }
};
```



## 2、目标和

#### [494. 目标和](https://leetcode.cn/problems/target-sum/)

正的和减去负的和即为目标值

正的和加上负的和等于sum

正 - 负 = target

正 + 负 = sum

正 = （target + sum ）/ 2（找出这么多个整数，要整除2）

求组合数的递推公式dp[j]=dp[j]+dp[j-nums[i]]

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int sum=0;
        for(int i=0;i<nums.size();i++) sum += nums[i];
        if((sum+target)%2 == 1 || abs(target)>sum) return 0;
        vector<int> dp((sum+target)/2+1,0);
        dp[0]=1;
        for(int i=0;i<nums.size();i++){
            for(int j=(sum+target)/2;j>=nums[i];j--){
                dp[j] += dp[j-nums[i]];
            }
        }
        return dp[(sum+target)/2];
    }
};
```



## 3、一和零

#### [474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)

遍历每个字符串

然后遍历大小为i×j的背包中放i个0和j个1所需的最大字符串个数

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m+1,vector<int>(n+1,0));
        for(string str:strs){
            int zeroNum=0,oneNum=0;
            for(char s:str){
                if(s == '0') zeroNum++;
                else oneNum++;
            }
            for(int i=m;i>=zeroNum;i--){
                for(int j=n;j>=oneNum;j--){
                    dp[i][j]=max(dp[i][j],dp[i-zeroNum][j-oneNum]+1);
                }
            }
        }
        return dp[m][n];
    }
};
```

