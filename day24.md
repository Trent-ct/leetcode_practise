# DAY24

## 1、组合

#### [77. 组合](https://leetcode.cn/problems/combinations/)

![image-20230324103243717](C:\Users\ct\AppData\Roaming\Typora\typora-user-images\image-20230324103243717.png)

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int n,int k,int startIndex){
        if(path.size()==k){
            result.push_back(path);
            return ;
        }
        for(int i=startIndex;i<=n;i++){//同层级遍历
            path.push_back(i);
            backtracking(n,k,i+1);//深度遍历
            path.pop_back();//回溯
        }
        return;
    }
public:
    vector<vector<int>> combine(int n, int k) {
        result.clear();
        path.clear();
        backtracking(n,k,1);
        return result;
    }
};
```



```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int n,int k,int startIndex){
        if(path.size()==k){
            result.push_back(path);
            return ;
        }
        //优化剪枝，如果剩下的元素个数已经不足以形成K个元素的组合了，就没有必要进行遍历
        for(int i=startIndex;i<=n-(k-path.size())+1;i++){//同层级遍历
            path.push_back(i);
            backtracking(n,k,i+1);//深度遍历
            path.pop_back();//回溯
        }
        return;
    }
public:
    vector<vector<int>> combine(int n, int k) {
        result.clear();
        path.clear();
        backtracking(n,k,1);
        return result;
    }
};
```

