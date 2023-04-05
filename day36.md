# DAY36

## 1、无重叠区间

#### [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)

与day35中的射爆气球想法类似，先对数组按照左边界从小到大排序

如果当前区间的左边界大于等于上一区间的右边界则表明没有重叠，循环继续

如果当前区间的左边界小于上一区间的右边界，则表明发生重叠，将当前区间和上一区间的最小右边界作为下一次比较的右边界

```c++
class Solution {
public:
    static bool cmp(const vector<int>& a,const vector<int>& b){
        if(a[0] == b[0]) return a[1]<b[1];
        return a[0]<b[0];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        int result=0;
        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]>=intervals[i-1][1]) continue;
            if(intervals[i][0]<intervals[i-1][1]){
                result++;
                intervals[i][1]=min(intervals[i][1],intervals[i-1][1]);
            }
        }
        return result;
    }
};
```



## 2、划分字母区间

#### [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/)

由于同一个字母只能出现在一个分段内，所以用数组哈希表记录下每个字母出现的最远的地方

遍历过程中不断更新右边界（当前分段内各个字母所能到达的最远边界），直到遍历到最远边界，进行切割

```c++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[26]={0};
        vector<int> result;
        for(int i=0;i<s.size();i++){
            hash[s[i]-'a'] = i;
        }
        int left=0;
        int right=0;
        for(int i=0;i<s.size();i++){
            right=max(right,hash[s[i]-'a']);
            if(i == right){
                result.push_back(right-left+1);
                left=i+1;
            }
        }
        return result;
    }
};
```



## 3、合并区间

#### [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)

当前区间的左边界与前面区间的最大右边界进行比较

若大于，则新开一个不重叠区间

若小于等于，则将当前区间融入到前面区间中去，更新最大右边界

```c++
class Solution {
public:
    static bool cmp(const vector<int>& a,const vector<int>& b){
        return a[0]<b[0];
    }
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        sort(intervals.begin(),intervals.end(),cmp);
        int left=intervals[0][0];
        int right=intervals[0][1];
        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]>right){
                result.push_back({left,right});
                left=intervals[i][0];
                right=intervals[i][1];
            }else{
                right=max(right,intervals[i][1]);
            }
        }
        result.push_back({left,right});
        return result;
    }
};
```

