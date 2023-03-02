# DAY2

## 1、有序数组的平方

##### **977.有序数组的平方**     https://leetcode.cn/problems/squares-of-a-sorted-array/

由于存在正负，数组的平方两头大中间小，双指针由两边向中间滑动的同时比较值的大小，有效进行平方的排序

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int size=nums.size();
        vector<int> nums_square(size,0);
        int left=0;
        int right=size-1;
        int i=size-1;
        while(left<=right){
            if(nums[left]*nums[left]>=nums[right]*nums[right]){
                nums_square[i]=nums[left]*nums[left];
                left++;
                i--;
            }else if(nums[left]*nums[left]<nums[right]*nums[right]){
                nums_square[i]=nums[right]*nums[right];
                right--;
                i--;
            }
        }
        return nums_square;
    }
};
```



## 2、长度最小的子数组

#####  209.长度最小的子数组    https://leetcode.cn/problems/minimum-size-subarray-sum/

起初只想到了暴力解法，但时间超时。

```c++

class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int size=nums.size();
        int length=size+1;
        for(int i=0;i<size;i++){
            int sum=0;
            for(int j=i;j<size;j++){
                sum=sum+nums[j];
                if(sum>=target&&j-i+1<length){
                    length=j-i+1;
                    break;
                }
            }
        }
        if(length<=size){
            return length;
        }
        else{
            return 0;
        }
    }
};
```

滑动窗口法，通过双指针锁定符合条件的窗口，找到后前指针不动，后指针前移，此时窗口再次不符合条件，前指针继续移动来寻找下一符合条件的窗口，过程中记录窗口信息，以此来进行比较得到最好的窗口。

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int size=nums.size();
        int length=size+1; //最终长度
        int dlength=0;   //长度计算中间量
        int sum=0;
        int j=0;
        for(int i=0;i<size;i++){
            sum+=nums[i];
            while(sum>=target){
                dlength=i-j+1;
                length=dlength<length?dlength:length;//通过比较更新最短长度
                sum-=nums[j++];
            }
        }
        if(length<=size){//小于等于size时表示找到了有效长度
            return length;
        }
        else{
            return 0;
        }
    }
};
```



## 3、螺旋数组Ⅱ

#####  59.螺旋矩阵II  https://leetcode.cn/problems/spiral-matrix-ii/

注意循环不变量，写的过程中会更加顺利，减少边界条件的考虑。起初想先确定循环次数，但发现n为奇数时中心点无法进入用左闭右开区间（循环不变量）写的循环中，需要左闭右闭才可以填上中心点，不如在后面另加判断来的简单快捷。

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n, 0));//数组定义
        int loop=n/2; //循环次数
        int center=n/2;//中心点
        int x_start=0;  //每条边的起点
        int y_start=0;
        int numb=1;//填入的数字
        int cutset=1;//边长缩短
        while(loop--){
            for(y_start;y_start<n-cutset;y_start++){//横前
                result[x_start][y_start]=numb++;
            }
            for(x_start;x_start<n-cutset;x_start++){//竖前
                result[x_start][y_start]=numb++;
            }
            for(y_start;y_start>-1+cutset;y_start--){//横回
                result[x_start][y_start]=numb++;
            }
            for(x_start;x_start>-1+cutset;x_start--){//竖回
                result[x_start][y_start]=numb++;
            }
            x_start++;
            y_start++;
            cutset++;
        }
        //补充n为奇数时中心点空缺
        if(n%2==1){
            result[center][center]=n*n;
        }
        return result;
    }
};
```

