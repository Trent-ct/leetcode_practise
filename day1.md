# DAY1

## 1、数组二分查找

##### 704.二分查找    https://leetcode.cn/problems/binary-search/

二分查找有效

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

## 2、数组移除元素

##### 27.移除元素    https://leetcode.cn/problems/remove-element/ 

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

