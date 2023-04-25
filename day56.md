# DAY56

## 1、两个字符串的删除操作

#### [583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/)

当前word1和word2的元素相同，则表明不需要删除

反之，取删除word1和word2一个的最小值

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        //动规数组表示需要删除的最少次数
        vector<vector<int>> dp(word1.size()+1,vector<int>(word2.size()+1));
        for(int i=0;i<=word1.size();i++) dp[i][0]=i;
        for(int j=1;j<=word2.size();j++) dp[0][j]=j;
        for(int i=1;i<=word1.size();i++){
            for(int j=1;j<=word2.size();j++){
                if(word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1];
                else dp[i][j]=min(dp[i-1][j]+1,dp[i][j-1]+1);
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```



## 2、编辑距离

#### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)

与上一题相比多了替换的操作

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size()+1,vector<int>(word2.size()+1));
        for(int i=0;i<=word1.size();i++) dp[i][0]=i;
        for(int j=1;j<=word2.size();j++) dp[0][j]=j;
        for(int i=1;i<=word1.size();i++){
            for(int j=1;j<=word2.size();j++){
                if(word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1];
                //按顺序分别为替换、删除、插入（1中插入等效于2中删除）
                else dp[i][j]=min(dp[i-1][j-1],min(dp[i-1][j],dp[i][j-1]))+1;
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```

