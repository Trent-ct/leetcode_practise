# DAY6

## 1、**有效的字母异位词** 

#####  **242.有效的字母异位词**     https://leetcode.cn/problems/valid-anagram/

将a-z 26个字母索引到下标为0-25的数组（a-z的ASCII连续），然后用索引后的数组去统计字符串中字母出现的次数。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26]={0};
        //记录字符串S中各个字母出现的次数
        for(int i=0;i<s.size();i++){
            record[s[i]-'a']++;
        }
        //字符串S中各字母出现次数减去T中出现的次数
        for(int i=0;i<t.size();i++){
            record[t[i]-'a']--;
        }
        //判断各个字母出现的次数是否为0，若为0则表示两个字符串所用字母一致
        for(int i=0;i<26;i++){
            if(record[i]!=0){
                return false;
            }
        }
        return true;
    }
};
```



## 2、两个数组的交集

#####  349. 两个数组的交集    https://leetcode.cn/problems/intersection-of-two-arrays/

求取两个数组的交集，不要求排序且要求去重，选用unordered_set作为数据结构，查询效率高且具备去重功能。先将数组1转换成一个哈希表，然后遍历数组2比对是否出现在数组1的哈希表中，若出现则插入到交集哈希表中。

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;//存储输出结果的set
        unordered_set<int> nums_set(nums1.begin(),nums1.end());//将数组1存入set中
        for(int i=0;i<nums2.size();i++){//可使用num:nums2来简化条件
            //判断数组2的值是否出现在nums_set中，若出现则插入至result_set中
            if(nums_set.find(nums2[i])!=nums_set.end()){
                result_set.insert(nums2[i]);
            }
        }
        return vector<int>(result_set.begin(),result_set.end());//返回结果
    }
};
```



## 3、快乐数

#####   **202. 快乐数**  https://leetcode.cn/problems/happy-number/

核心思想在于判断各位平方和是否出现循环，利用哈希表存储过往平方和。选用unordered_set提升查询效率。

```c++
class Solution {
public:
    int getsum(int n){//求取各位平方和
        int sum=0;
        while(n){
            sum+=(n%10)*(n%10);
            n=n/10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> set;
        while(1){
            int sum=getsum(n);
            if(sum==1) return true;
            if(set.find(sum)!=set.end()){//判断是否出现重复
                return false;
            }else{
                set.insert(sum);//插入新值
            }
            n=sum;
        }
    }
};
```

## 4、两数之和

#####  **1. 两数之和**  https://leetcode.cn/problems/two-sum/

两数之和使用哈希表中的map数据结构，由于需要存储值和下标，而map可以同时存储key和value。将数组的值作为key，下表作为value，因为查询时需要用数组的值，而返回的时需要数组的下标。

map的主要作用是存储已经遍历过的元素，遍历新的元素时可以判断已经遍历过的元素中是否存在满足两数之和等于target的元素，本质上是一种查询。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> map;
        for(int i=0;i<nums.size();i++){
            auto iter=map.find(target-nums[i]);
            if(iter!=map.end()){  //判断是否有已经存储过的元素
                return {iter->second,i};
            }
            map.insert(pair<int,int>(nums[i],i));//在map中插入元素
        }
        return {};
    }
};
```

