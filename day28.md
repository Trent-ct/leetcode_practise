# DAY28

## 1、复原 IP 地址

#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

对字符串进行切割得到正确的IP地址



```c++
class Solution {
private:
    vector<string> result;
    string path;
    //判断字符段是否满足要求
    bool isValid(const string& s, int left, int right) {
        if (left > right) {
            return false;
        }
        if (s[left] == '0' && left != right) {
            return false;
        }

        int sum = 0;
        for (int i = left; i <= right; i++) {
            if (s[i] > '9' || s[i] < '0') {
                return false;
            }
            sum = sum * 10 + (s[i] - '0');
            if (sum > 255) {
                return false;
            }
        }
        return true;
    }

    void backtracking(const string& s, int startIndex, int pointNum) {
        if (pointNum == 3) {//如果已有三个点，判断剩余的字符串是否满足要求
            if (isValid(s, startIndex, s.size() - 1)) {//若满足要求，将path添加到结果中
                result.push_back(path + s.substr(startIndex));
            }
            return;
        }

        for (int i = startIndex; i < s.size(); i++) {
            if (isValid(s, startIndex, i)) {//如果满足要求，进入下一层切割
                string str=s.substr(startIndex,i-startIndex+1);
                path += str;
                path += ".";
                pointNum++;
                backtracking(s, i + 1, pointNum);
                pointNum--;
                path.resize(path.size() - str.size() - 1);
            } else {
                break;
            }
        }
    }

public:
    vector<string> restoreIpAddresses(string s) {
        result.clear();
        path.clear();
        if(s.size()<4||s.size()>12) return result;//剪枝一下
        backtracking(s, 0, 0);
        return result;
    }
};

```



## 2、子集

#### [78. 子集](https://leetcode.cn/problems/subsets/)

与组合问题不同的是，每次递归都要收集结果

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int startIndex){
        //每一次递归都要收集结果
        result.push_back(path);

        for(int i=startIndex;i<nums.size();i++){
            path.push_back(nums[i]);
            backtracking(nums,i+1);
            path.pop_back();
        }

    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums,0);
        return result;
    }
};
```



## 3、子集 II

#### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)

由于数组中存在相同元素，故需要进行去重

将数组排序后再进行回溯

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums,int startIndex){
        result.push_back(path);
        for(int i=startIndex;i<nums.size();i++){
            if(i>startIndex && nums[i]==nums[i-1]) continue;
            path.push_back(nums[i]);
            backtracking(nums,i+1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(),nums.end());
        backtracking(nums,0);
        return result;
    }
};
```

