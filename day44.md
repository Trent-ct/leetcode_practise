# DAY44

## 1、零钱兑换 II

#### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)

完全背包问题

求取组合数 递推公式为dp[j]+=dp[j-coins[i]]

遍历顺序为先物品再背包

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount+1,0);
        dp[0]=1;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++){
                dp[j] += dp[j-coins[i]];
            }
        }
        return dp[amount];
    }
};
```



## 2、组合总和 Ⅳ

#### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)

完全背包求排列数

先遍历背包然后遍历物品

注意组合数超限的问题，限制在INT_MAX内

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target+1,0);
        dp[0]=1;
        for(int i=0;i<=target;i++){
            for(int j=0;j<nums.size();j++){
                if(i - nums[j] >= 0&&dp[i] < INT_MAX - dp[i - nums[j]]){
                    dp[i]+=dp[i-nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```



