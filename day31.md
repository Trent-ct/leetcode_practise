# DAY31

## 1、分发饼干

#### [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)

小饼干优先喂饱小胃口，如果当前饼干无法喂饱对应小胃口，就说明饼干小了，加大饼干来满足胃口

```c++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int index=0;
        for(int i=0;i<s.size();i++){
            if(index<g.size()&&g[index]<=s[i]){
                index++;
            }
        }
        return index;
    }
};
```





## 2、摆动序列

#### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

核心思想在于去除单调序列，只留下波峰和波谷

```c++
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()<=1) return nums.size();
        int prediff=0;
        int curdiff=0;
        int result=1;
        for(int i=0;i<nums.size()-1;i++){
            curdiff=nums[i+1]-nums[i];
            if((prediff<=0&&curdiff>0)||(prediff>=0&&curdiff<0)){
                result++;
                prediff=curdiff;
            }
        }
        return result;
    }
};
```



## 3、最大子数组和

#### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

累计sum值，当sum小于等于0时，从下一位开始的累计的sum值一定大于等于继续累计的sum值

所以当sum小于等于0时，从下一为开始重新累计sum值

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result=INT_MIN;
        int sum=0;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
            if(sum>result){
                result=sum;
            }
            if(sum<=0){
                sum=0;
            }
        }
        return result;
    }
};
```

