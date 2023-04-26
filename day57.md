# DAY57

## 1、回文子串

#### [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)

若s[i]和s[j]相同，那么只要i-1和j+1之间的字符串为回文子串，i和j之间的也为回文子串

```c++
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(),vector<bool>(s.size(),false));
        int result=0;
        for(int i=0;i<s.size();i++){
            for(int j=i;j>=0;j--){
                if(s[i]==s[j]&&(i-j<=1||dp[i-1][j+1])){
                    result++;
                    dp[i][j]=true;
                }
            }
        }
        return result;
    }
};
```



## 2、最长回文子序列

#### [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

s[i]与s[j]相同时，可以向最长子序列中添加两个元素

不相同时，只能选择添加一个元素后取较长的子序列

```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(),vector<int>(s.size(),0));
        for(int i=0;i<s.size();i++) dp[i][i]=1;
        for(int i=1;i<s.size();i++){
            for(int j=i-1;j>=0;j--){
                if(s[i]==s[j]) dp[i][j]=dp[i-1][j+1]+2;
                else dp[i][j]=max(dp[i-1][j],dp[i][j+1]);
            }
        }
        return dp[s.size()-1][0];
    }
};
```

