# DAY49

## 1、买卖股票的最佳时机

#### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

只买入一次，卖出一次

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len=prices.size();
        if(len==1) return 0;
        vector<vector<int>> dp(len,vector<int>(2,0));
        //0表示不持有股票，1表示持有股票
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        for(int i=1;i<len;i++){
            //当前不持有，要么本来就不持有或者当前卖出
            dp[i][0]=max(prices[i]+dp[i-1][1],dp[i-1][0]);
            //当前持有，当前买入或者之前就持有
            dp[i][1]=max(-prices[i],dp[i-1][1]);
        }
        return dp[len-1][0];
    }
};
```



## 2、买卖股票的最佳时机 II

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

同一支股票可以多次买入卖出

但需要卖出后才能再次买入

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len=prices.size();
        if(len==1) return 0;
        vector<vector<int>> dp(len,vector<int>(2));
        //0表示当前不持有股票，1表示当前持有股票
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        for(int i=1;i<len;i++){
            dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i]);
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i]);
        }
        return dp[len-1][0];
    }
};
```



打卡
