# DAY58

## 1、每日温度

#### [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

单调栈

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1,vector<int>(text2.size()+1,0));
        int result=0;
        for(int i=1;i<=text1.size();i++){
            for(int j=1;j<=text2.size();j++){
                if(text1[i-1]==text2[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                if(dp[i][j]>result) result=dp[i][j];
            }
        }
        return result;
    }
};
```



## 2、下一个更大元素 I

#### [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

由于数组1的元素都在数组2中，所以按照上一题的处理方式处理数组2，然后每当遇到了数组1中的元素就输出一下

```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> st;
        vector<int> result(nums1.size(),-1);

        unordered_map<int,int> umap;
        for(int i=0;i<nums1.size();i++){
            umap[nums1[i]]=i;
        }

        st.push(0);
        for(int i=1;i<nums2.size();i++){
            if(nums2[i]<=nums2[st.top()]){
                st.push(i);
            }else{
                while(!st.empty()&&nums2[i]>nums2[st.top()]){
                    if(umap.find(nums2[st.top()])!=umap.end()){
                        result[umap[nums2[st.top()]]]=nums2[i];
                    }
                    st.pop();
                }
                st.push(i);
            }
        }
        return result;
    }
};
```
