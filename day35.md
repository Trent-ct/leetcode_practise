# DAY35

## 1、柠檬水找零

#### [860. 柠檬水找零](https://leetcode.cn/problems/lemonade-change/)

表面是一道模拟题，注意在给20找零时先选择10+5，再选择5+5+5

```c++
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five=0,ten=0,twenty=0;
        for(int i=0;i<bills.size();i++){
            if(bills[i] == 5){
                five++;
            }
            if(bills[i] == 10){
                ten++;
                five--;
                if(five < 0) return false;
            }
            if(bills[i]== 20){
                twenty++;
                if(ten > 0){
                    ten--;
                    five--;
                    if(five < 0) return false;
                }else{
                    five=five-3;
                    if(five < 0) return false;
                }
            }
        }
        return true;
    }
};
```



## 2、根据身高重建队列

#### [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)

使用list作为容器去做插入操作，节约时间

```c++
class Solution {
public:
    static bool cmp(const vector<int>& a,const vector<int>& b){
        if(a[0]==b[0]) return a[1]<b[1];
        return a[0]>b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        list<vector<int>> list_que;
        for(int i=0;i<people.size();i++){
            int position=people[i][1];
            auto it=list_que.begin();
            while(position--){
                it++;
            }
            list_que.insert(it,people[i]);

        }
        //将list中的元素复制到一个新的vector容器中
        return vector<vector<int>> (list_que.begin(),list_que.end());
    }
};
```



## 3、用最少数量的箭引爆气球

#### [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)

每次与上一个气球比较完之后需要更新当前气球的边界

由于同一支箭的有效范围只到上一个气球的右边界，如果当前气球的左边界小于前一气球的右边界，那么可以由同一支箭射爆，但有效范围也只到前一气球的右边界，所以需要将当前气球的右边界与前一气球的右边界比较，取边界较大的作为当前气球的新右边界。

```c++
class Solution {
public:
    static bool cmp(const vector<int>& a,const vector<int>& b){
        return a[1]<b[1];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        int result=1;
        sort(points.begin(),points.end(),cmp);
        for(int i=1;i<points.size();i++){
            if(points[i][0]>points[i-1][1]){
                result++;
            }else{
                points[i][1]=min(points[i][1],points[i-1][1]);//确定最小边界
            }
        }
        return result;
    }
};
```

