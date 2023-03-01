# DAY1

## 1、数组二分查找

##### 704.二分查找    https://leetcode.cn/problems/binary-search/

二分查找通过不断判断元素在二分区间的哪半边来提高搜索效率，左闭右闭和左闭右开的区别着重于右端点的范围和判断

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=0;
        int right=nums.size()-1;
        while(left<=right){                //左闭右闭区间
            int middle=left+(right-left)/2;
            if(nums[middle]>target){
                 right=middle-1;
            }
            else if(nums[middle]<target){
                left=middle+1;
            }
            else{
               return middle; 
            }
        }
        return -1;
    }
};
```

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left=0;
        int right=nums.size();
        while(left<right){          //左闭右开
            int middle=left+(right-left)/2;
            if(nums[middle]>target){
                 right=middle;
            }
            else if(nums[middle]<target){
                left=middle+1;
            }
            else{
               return middle; 
            }
        }
        return -1;
    }
};
```

##### 35.搜索插入位置  https://leetcode.cn/problems/search-insert-position/

在二分查找的基础上，循环外部多加一处判断，来确定插入位置（元素可能比middle处值大，也有可能比middle处值小）

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left=0;
        int right=nums.size()-1;
        int middle=left+(right-left)/2;
        while(left<=right){     //左闭右闭二分查找
            middle=left+(right-left)/2;
            if(nums[middle]>target){
                right=middle-1;
            }
            else if(nums[middle]<target){
                left=middle+1;
            }
            else{
                return middle;
            }
        }
        //判断插入位置
        if(nums[middle]>target){
            return middle;
        }
        else {
            return middle+1;
         }
    }
};
```



## 2、数组移除元素

##### 27.移除元素    https://leetcode.cn/problems/remove-element/ 

暴力解法中，每找到一个需要移除的元素，就用数组后续元素向前覆盖一位

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i=0;
        int size=nums.size();
        for(i=0;i<size;i++){
            if(nums[i]==val){
                for(int j=i;j<size-1;j++){
                    nums[j]=nums[j+1];
                }
                i--;
                size--;
            }
        }
        return size;
    }
};
```

双指针思路中，慢指针等待快指针找到下一个无需删除的元素，然后进行元素覆盖

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow=0;
        int fast=0;
        for(fast=0;fast<nums.size();fast++){
            if(val!=nums[fast]){
                nums[slow++]=nums[fast];
            }
        }
        return slow;
    }
};
```

