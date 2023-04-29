# DAY59

## 1、下一个更大元素 II

#### [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)

循环中遍历两边数组

```c++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<int> st;
        vector<int> result(nums.size(),-1);
        st.push(0);
        for(int i=1;i<2*nums.size();i++){
            if(nums[i%nums.size()]<=nums[st.top()]){
                st.push(i%nums.size());
            }else {
                while(!st.empty()&&nums[i%nums.size()]>nums[st.top()]){
                    result[st.top()]=nums[i%nums.size()];
                    st.pop();
                }
                st.push(i%nums.size());
            }
        }
        return result;
    }
};
```



## 2、接雨水

#### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

使用单调栈，按照行来计算雨水量

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int> st;
        int result=0;
        st.push(0);
        for(int i=1;i<height.size();i++){
            if(height[i]<height[st.top()]){
                st.push(i);
            }else if(height[i]==height[st.top()]){
                st.pop();
                st.push(i);
            }else{
                while(!st.empty()&&height[i]>height[st.top()]){
                    int tmp=st.top();
                    st.pop();
                    if(!st.empty()){
                        int h=min(height[i],height[st.top()])-height[tmp];
                        int w=i-st.top()-1;
                        result += h*w;
                    }
                }
                st.push(i);
            }
        }
        return result;
    }
};
```

