# DAY46

## 1、单词拆分

#### [139. 单词拆分](https://leetcode.cn/problems/word-break/)

每次拆分的单词就是物品，长度为i的字符串为背包

如果当前长度减去拆分的长度的字符串为true，那么只要拆分出的单词也在字典中，当前长度的字符串也为true

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordset(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.size()+1,false);
        dp[0]=true;
        for(int i=1;i<=s.size();i++){
            for(int j=0;j<i;j++){
                string word=s.substr(j,i-j);
                if(wordset.find(word)!=wordset.end()&&dp[j])
                dp[i]=true;
            }
        }
        return dp[s.size()];
    }
};
```



