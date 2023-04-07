# DAY38

## 1、斐波那契数

#### [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

```c++
class Solution {
public:
    int fib(int n) {
        if(n <= 1) return n;
        int fib_num[n+1];
        //初始值
        fib_num[0]=0;
        fib_num[1]=1;
        for(int i=2;i<=n;i++){
            fib_num[i]=fib_num[i-1]+fib_num[i-2];//当前值由前两位值相加而来，状态转移
        }
        return fib_num[n];
    }
};
```



## 2、爬楼梯

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

由于每次都只能跨越1或2个台阶，那么当前楼层的到达方式取决于前一个台阶的到达方式加上前两个台阶的到达方式。初始化从第一个台阶开始

```c++
class Solution {
public:
    int climbStairs(int n) {
        if(n<=1) return n;
        int dp[n+1];
        dp[1]=1;
        dp[2]=2;
        for(int i=3;i<=n;i++){
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
};
```



## 3、使用最小花费爬楼梯

#### [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

从0或1出发，注意目的地是cost最大索引值的后一位，所以dp大小为cost.size（）+1

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int dp[cost.size()+1];
        dp[0]=0;
        dp[1]=0;
        for(int i=2;i<=cost.size();i++){
            dp[i]=min(dp[i-2]+cost[i-2],dp[i-1]+cost[i-1]);
        }
        return dp[cost.size()];
    }
};
```

