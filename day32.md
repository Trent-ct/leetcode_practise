# DAY32

## 1、买卖股票的最佳时机 II

#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

并不限制股票的售卖此处，所以只需要记录所有涨的区间（大小为2）差值

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int max=0;
        for(int i=0;i<prices.size()-1;i++){
            if((prices[i+1]-prices[i])>0){
                max+=prices[i+1]-prices[i];
            }
        }
        return max;
    }
};
```





## 2、跳跃游戏

#### [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

核心在于计算当前节点所能到达的最大范围，若最大范围没有覆盖到终点

那么就遍历最大范围内的下一个节点

更新最大范围

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover=0;
        if(nums.size()==1) return true;
        for(int i=0;i<=cover;i++){
            cover=max(i+nums[i],cover);//每走一步就计算新的元素能到达的范围是否超过了之前的范围
            if(cover>=nums.size()-1) return true;
        }
        return false;
    }
};
```



## 3、跳跃游戏 II

#### [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/)

每次遍历到上一次更新的最大范围后，就需要加一次跳跃步骤，并更新覆盖范围

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int curCover=0;
        int nextCover=0;
        int jumpNum=0;
        for(int i=0;i<=nums.size()-2;i++){
            nextCover=max(i+nums[i],nextCover);
            if(i==curCover){
                if(curCover<nums.size()-1){
                    jumpNum++;
                    curCover=nextCover;
                    if(nextCover>=nums.size()-1) break;
                }else break;
            }
        }
        return jumpNum;
    }
};
```

