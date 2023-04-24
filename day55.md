# DAY54

## 1、判断子序列

#### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

与求公共子序列不同的是，只删除长序列中的元素

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        vector<vector<int>> dp(s.size()+1,vector<int>(t.size()+1,0));
        for(int i=1;i<=s.size();i++){
            for(int j=1;j<=t.size();j++){
                if(s[i-1]==t[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                if(s[i-1]!=t[j-1]) dp[i][j]=dp[i][j-1];
            }
        }
        return dp[s.size()][t.size()]==s.size();
    }
};
```



## 2、不同的子序列

#### [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

注意动规数组范围

```c++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint64_t>> dp(s.size()+1,vector<uint64_t>(t.size()+1,0));
        for(int i=0;i<=s.size();i++) dp[i][0]=1;
        for(int i=1;i<=s.size();i++){
            for(int j=1;j<=t.size();j++){
                if(s[i-1]==t[j-1]) dp[i][j]=dp[i-1][j-1]+dp[i-1][j];
                else dp[i][j]=dp[i-1][j];
            }
        }
        return dp[s.size()][t.size()];
    }
};
```


