# DAY27

## 1、组合总和

#### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

元素可以重复取

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int sum,int target,int startIndex){
        if(sum>target) return ;
        else if(sum==target){
            result.push_back(path);
            return ;
        }
        for(int i=startIndex;i<nums.size();i++){
            path.push_back(nums[i]);
            backtracking(nums,sum+nums[i],target,i);//由于元素可以重复取值，所以startIndex为i
            path.pop_back();
        }
    }    
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        backtracking(candidates,0,target,0);
        return result;
    }
};
```



## 2、组合总和 II

#### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)

要对结果集进行去重

先对candidates进行排序，然后若相同层级的下一个取值与上一个相同则跳过

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int sum,int target,int startIndex){
        if(sum>target) return ;
        else if(sum==target){
            result.push_back(path);
            return ;
        }
        for(int i=startIndex;i<nums.size();i++){
            if(i>startIndex&&nums[i]==nums[i-1]) continue;//进行去重
            path.push_back(nums[i]);
            backtracking(nums,sum+nums[i],target,i+1);
            path.pop_back();
        }
    }    
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        sort(candidates.begin(),candidates.end());
        backtracking(candidates,0,target,0);
        return result;
    }
};
```



## 3、分割回文串

#### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)



```c++
class Solution {
private:
    vector<vector<string>> result;
    vector<string> path;
    //判断是否为回文子串
    bool isPalindrome(string& s,int left,int right){
        for(int i=left,j=right;i<j;i++,j--){
            if(s[i]!=s[j]) return false;
        }
        return true;
    }
    //回溯切割
    void backtracking(string& s,int startIndex){
        if(startIndex>=s.size()){//注意终止条件
            result.push_back(path);
            return ;
        }
        for(int i=startIndex;i<s.size();i++){
            //直到找到一个回文子串，切割后进入递归
            if(isPalindrome(s,startIndex,i)){
                string str=s.substr(startIndex,i-startIndex+1);
                path.push_back(str);
            }else{
                continue;
            }
            backtracking(s,i+1);
            path.pop_back();

        }

    } 
public:
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s,0);
        return result;
    }
};
```

