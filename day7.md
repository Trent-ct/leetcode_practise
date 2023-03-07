# DAY7

## 1、四数相加Ⅱ

#####  **454.四数相加II **     https://leetcode.cn/problems/4sum-ii/

注意需要用一个变量来记录所有和为0的元组数

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> map;
        for(int a:nums1){
            for(int b:nums2){
                map[a+b]++;
            }
        }
        int count=0;//记录和为0的次数
        for(int c:nums3){
            for(int d:nums4){
                auto iter=map.find(0-c-d);
                if(iter!=map.end()){
                    count+=iter->second;
                }
            }
        }
        return count;
    }
};
```



## 2、赎金信

#####  383.赎金信     https://leetcode.cn/problems/ransom-note/

使用数组实现哈希表，下文所示代码中第三个判断循环可以直接加入到第二个循环中去，有效节省时间

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int hash[26]={0};
        for(int i=0;i<magazine.size();i++){//记录magazine中各字母的个数
            hash[magazine[i]-'a']++;
        }
        for(int i=0;i<ransomNote.size();i++){//ransomnote每使用一个字母就减一
            hash[ransomNote[i]-'a']--;
        }
        for(int i=0;i<26;i++){//若hash表中有元素小于0，则表示不能
            if(hash[i]<0){
                return false;
            }
        }
        return true;
    }
};
```



## 3、三数之和

#####   15.**三数之和**  https://leetcode.cn/problems/3sum/

使用双指针计算等于0的三数之和，关键在于给元素去重（判断i的元素是否和i-1的元素相等）

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int i,left,right;
        sort(nums.begin(),nums.end());//对数组进行排序
        for(i=0;i<nums.size();i++){
            if(nums[i]>0) break;//第一个元素就大于0，则表明无论后续无论如何相加都大于0
            if(i>0&&nums[i]==nums[i-1]) continue;//给i去重
            left=i+1;//左指针
            right=nums.size()-1;//右指针
            while(left<right){
                if(left>i+1&&nums[left]==nums[left-1]){//给左指针去重
                    left++;
                    continue;
                }
                if(right<(nums.size()-1)&&nums[right]==nums[right+1]){
                    //给右指针去重
                    right--;
                    continue;
                }
                if(nums[i]+nums[left]+nums[right]==0){//满足条件插入至结果内
                    result.push_back(vector<int>{nums[i],nums[left],nums[right]});
                    left++;
                    right--;
                }else if(nums[i]+nums[left]+nums[right]>0){//大于0时缩小三元组的和
                    right--;
                }else{//小于0时，增大三元组的和
                    left++;
                }
            }
        }
        return result;
    }
};
```

## 4、四数之和

#####  **16.四数之和**  https://leetcode.cn/problems/two-sum/

四数之和在三数之和的基础上增加一层循环，内核依旧是使用双指针方法。

其中需要注意的是去重和剪枝

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        int i,j,left,right;
        sort(nums.begin(),nums.end());//对数组进行排序
        for(i=0;i<nums.size();i++){
            if(nums[i]>target&&nums[i]>0) break;//剪枝
            if(i>0&&nums[i]==nums[i-1]) continue;//去重
            for(j=i+1;j<nums.size();j++){
                if(nums[i]+nums[j]>target&&nums[i]+nums[j]>0) break;//剪枝
                if(j>i+1&&nums[j]==nums[j-1]) continue;//去重
                left=j+1;//左指针
                right=nums.size()-1;//右指针
                while(left<right){
                    //左指针去重
                    if(left>j+1&&nums[left]==nums[left-1]){
                        left++;
                        continue;
                    }
                    //右指针去重
                    if(right<nums.size()-1&&nums[right]==nums[right+1]){
                        right--;
                        continue;
                    }
                    //大于target时右指针左移，小于target时左指针右移，等于target时插入至result
                    if((long)nums[i]+nums[j]+nums[left]+nums[right]>target){
                        right--;
                    }else if((long)nums[i]+nums[j]+nums[left]+nums[right]<target){
                        left++;
                    }else{
                        result.push_back(vector<int>{nums[i],nums[j],nums[left],nums[right]});
                        left++;
                        right--;
                    }
                }

            }
        }
        return result;
    }
};
```

