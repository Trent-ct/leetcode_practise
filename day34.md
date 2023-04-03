# DAY34

## 1、K 次取反后最大化的数组和

#### [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

先对绝对值较大的负数进行取反操作，尽可能增大和

若操作次数仍有剩余，则对绝对值最小的数进行不断的取反操作，直到操作数为0，尽可能减小对和的影响

```c++
class Solution {
    static bool cmp(int a,int b){
        return abs(a)>abs(b);
    }
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end(),cmp);
        for(int i=0;i<nums.size();i++){
            if(nums[i] < 0 && k>0){
                nums[i]=nums[i]*(-1);
                k--;
            }
        }
        if(k % 2 == 1) nums[nums.size()-1] = (-1)*nums[nums.size()-1];
        int result=0;
        for(int a:nums){
            result += a;
        }
        return result;
    }
};
```





## 2、加油站

#### [134. 加油站](https://leetcode.cn/problems/gas-station/)

核心在于利用好gas和cost的差

如果gas总和小于cost总和，那就说明加的油满足不了消耗的油

每次计算从startIndex到当前节点gas-cost的和，如果该和为负，那就说明startIndex无法到达当前节点，且startIndex到当前节点中的任意节点都无法到达当前节点，从当前节点的下一节点重新开始



```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum=0;
        int totalSum=0;
        int startIndex=0;
        for(int i=0;i<gas.size();i++){
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if(curSum < 0){
                curSum = 0;
                startIndex = i+1;
            }
        }

        if(totalSum < 0) return -1;
        return startIndex;
    }
};
```



## 3、分发糖果

#### [135. 分发糖果](https://leetcode.cn/problems/candy/)

先从左到右遍历评分表，如果右大于左，那么右边的糖果数量为左的+1

再从右到左遍历评分表，如果左大于右，那么左的糖果数量应为右的+1，但如果当前左的糖果数量已经满足左的大于右的，那么就保持原样，否则可能无法满足左大于左的左

```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> vecCandy(ratings.size(),1);
        for(int i=1;i<ratings.size();i++){
            if(ratings[i]>ratings[i-1]) vecCandy[i] = vecCandy[i-1] + 1;
        }
        for(int i=ratings.size()-2;i>=0;i--){
            if(ratings[i]>ratings[i+1])
            vecCandy[i]=max(vecCandy[i],vecCandy[i+1]+1);
       }
        int result=0;
        for(auto a:vecCandy) result += a;
        return result;
    }
};
```

