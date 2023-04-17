# DAY45

## 1、爬楼梯

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)（进阶）

每次可以爬1-m个台阶

```c++
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) { // 遍历背包
            for (int j = 1; j <= m; j++) { // 遍历物品
                if (i - j >= 0) dp[i] += dp[i - j];
            }
        }
        return dp[n];
    }
};
```



## 2、零钱兑换

#### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)

找的是最少硬币数，所以递推中是min函数

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1,INT_MAX);
        dp[0]=0;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++){
                if(dp[j-coins[i]]!=INT_MAX){
                    dp[j]=min(dp[j-coins[i]]+1,dp[j]);
                }
            }
        }
        if(dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```



## 3、完全平方数

#### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

与上一题类似，找的是最小组合

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0]=0;
        for(int i=1;i*i<=n;i++){
            for(int j=i*i;j<=n;j++){
                dp[j]=min(dp[j-i*i]+1,dp[j]);
            }
        }
        return dp[n];
    }
};
```

