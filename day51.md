# DAY51

## 1、最佳买卖股票时机含冷冻期

#### [309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

考虑新状态冷冻期

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()<=1) return 0;
        vector<vector<int>> dp(prices.size(),vector<int>(4));
        //0表示买入状态，1表示已经卖出状态，2表示当前卖出状态，3表示当前为冷冻期
        dp[0][0]=-prices[0];
        dp[0][1]=0;
        dp[0][2]=0;
        dp[0][3]=0;
        for(int i=1;i<prices.size();i++){
            dp[i][0]=max(dp[i-1][0],max(dp[i-1][1]-prices[i],dp[i-1][3]-prices[i]));
            dp[i][1]=max(dp[i-1][1],dp[i-1][3]);
            dp[i][2]=dp[i-1][0]+prices[i];
            dp[i][3]=dp[i-1][2];
        }
        return max(dp[prices.size()-1][1],max(dp[prices.size()-1][2],dp[prices.size()-1][3]));
    }
};
```



## 2、买卖股票的最佳时机含手续费

#### [714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

买入时多支付一次手续费

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int len=prices.size();
        if(len <= 1) return 0;
        vector<vector<int>> dp(len,vector<int>(2));
        //0表示当前不持有股票，1表示当前持有股票
        dp[0][0]=0;
        dp[0][1]=-prices[0]-fee;
        for(int i=1;i<len;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i]-fee);
        }
        return dp[len-1][0];
    }
};
```



