# DAY29

## 1、递增子序列

#### [491. 递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/)



```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int startIndex){
        if(path.size() > 1){
            result.push_back(path);
        }
        //用哈希表来存储已经用过的元素，同一层级中使用过的元素就直接跳过
        unordered_set<int> used;
        for(int i=startIndex;i<nums.size();i++){
            if((!path.empty() && nums[i]<path.back())||used.find(nums[i])!=used.end()){
                continue;
            }
            used.insert(nums[i]);
            path.push_back(nums[i]);
            backtracking(nums,i+1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums,0);
        return result;
    }
};
```



## 2、全排列

#### [46. 全排列](https://leetcode.cn/problems/permutations/)

树枝往下遍历时，若元素已被使用过就不能再次使用

与组合不同的是，组合中每次从新的startIndex开始，自动去除了已经使用过的元素

而排列则需要对数组中使用过的元素进行去除

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,vector<bool> used){
        if(path.size()==nums.size()){
            result.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(used[i]==true){
                continue;
            }
            used[i]=true;
            path.push_back(nums[i]);
            backtracking(nums,used);
            path.pop_back();
            used[i]=false;

        }

    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(),false);
        backtracking(nums,used);
        return result;
    }
};
```



## 3、全排列 II

#### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)

使用set去重

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,vector<bool> used){
        if(path.size()==nums.size()){
            result.push_back(path);
            return;
        }
        unordered_set<int> uset;//对树层去重
        //used对树枝去重
        for(int i=0;i<nums.size();i++){
            if(uset.find(nums[i])!=uset.end()||used[i]==true){
                continue;
            }
            uset.insert(nums[i]);
            used[i]=true;
            path.push_back(nums[i]);
            backtracking(nums,used);
            path.pop_back();
            used[i]=false;
        }

    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(),false);
        backtracking(nums,used);
        return result;
    }
};
```

使用数组去重

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {
        // 此时说明找到了一组
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end()); // 排序
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

