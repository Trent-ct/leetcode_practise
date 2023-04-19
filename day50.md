# DAY50

## 1、买卖股票的最佳时机 III

#### [123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

至多买卖两次股票

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len=prices.size();
        if(len==1) return 0;
        vector<vector<int>> dp(len,vector<int>(5));
        //0表示无操作，1表示第一次买入，2表示第一次卖出，3表示第二次买入,4表示第2次卖出
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        dp[0][2]=0;
        dp[0][3]=-prices[0];
        dp[0][4]=0;
        for(int i=1;i<len;i++){
            dp[i][0]=dp[i-1][0];
            dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i]);
            dp[i][2]=max(dp[i-1][2],dp[i-1][1]+prices[i]);
            dp[i][3]=max(dp[i-1][3],dp[i-1][2]-prices[i]);
            dp[i][4]=max(dp[i-1][4],dp[i-1][3]+prices[i]);
        }
        return dp[len-1][4];
    }
};
```



## 2、买卖股票的最佳时机 IV

#### [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

从上一题中总结规律，得到K次买入卖出的递推公式

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        if(prices.size()==0) return 0;
        vector<vector<int>> dp(prices.size(),vector<int>(2*k+1,0));
        for(int i=1;i<=2*k-1;i += 2){
            dp[0][i]=-prices[0]; 
        }
        //0不操作，1买入，2卖出，3买入，4卖出，...
        for(int i=1;i<prices.size();i++){
            for(int j=1;j<=2*k-1;j+=2){
                dp[i][j]=max(dp[i-1][j],dp[i-1][j-1]-prices[i]);
                dp[i][j+1]=max(dp[i-1][j+1],dp[i-1][j]+prices[i]);
            }
        }
        return dp[prices.size()-1][2*k];
    }
};
```


