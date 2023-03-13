# DAY13

## 1、滑动窗口最大值

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

使用数据结构deque，双端队列容器，可以在首位进行插入删除操作

维护一个单调队列，存储滑动窗口中的最大值、次大值、...

队列头就是每个滑动窗口的最大值

```c++
class Solution {
private:
    class MyQueue{
    public:
           deque<int> que;
           void pop(int value){//当窗口后移一位时，移除前一位的值（若为最大值）
               if(!que.empty()&&value==que.front()){
                   que.pop_front();
               }
           }
           void push(int value){//插入值时要与队列中前面的值进行比较，直到其排在比它大的值后面
               while(!que.empty()&&value>que.back()){
                   que.pop_back();
               }
               que.push_back(value);
           }
           int que_max(){//队列头就是窗口最大值
               return que.front();
           }

    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;//单调队列
        vector<int> result;
        for(int i=0;i<k;i++){//将起始窗口插入到单调队列中
            que.push(nums[i]);
        }
        result.push_back(que.que_max());//第一个最大值
        for(int i=k;i<nums.size();i++){//窗口开始移动，每次移动要删除一个值，插入一个值
            que.pop(nums[i-k]);
            que.push(nums[i]);
            result.push_back(que.que_max());
        }
        return result;
    }
};
```



## 2、前K个高频元素

#### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

将数组存储为map，保存每个值以及其出现的次数，利用优先级队列对map进行排序

使用小顶堆优先级队列，保留前K个高频元素的<key，value>对，最后倒序输出Key值

```c++
class Solution {
public:
    //小顶堆仿函数
    class mycompare{
        public:
        bool operator()(const pair<int,int>& lhs,const pair<int,int>& rhs){
            return lhs.second>rhs.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> map;
        for(int i=0;i<nums.size();i++){
            map[nums[i]]++;
        }
        //定义优先级队列（小顶堆）
        priority_queue<pair<int,int>,vector<pair<int,int>>,mycompare> pri_que;
        //将map输入到小顶堆的优先级队列中，只保留value值较大的K项，代表频率高
        for(auto it=map.begin();it!=map.end();it++){//注意使用迭代器进行循环
            pri_que.push(*it);
            if(pri_que.size()>k){
                pri_que.pop();
            }
        }
        //输出频率高的map中的key值
        vector<int> result(k);
        for(int i=k-1;i>=0;i--){
            result[i]=pri_que.top().first;
            pri_que.pop();
        }
        return result;


    }
};
```

