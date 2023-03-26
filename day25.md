# DAY25

## 1、组合总和 III

#### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

注意剪枝优化

```c++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int k, int n,int sum,int startIndex){
        if(sum>n) return;//和已经大于目标值，无论数有几个都没有意义
        if(path.size()==k){
            if(sum==n){
                result.push_back(path);//仅当和为目标值时，将path压入结构
            }
            return ;
        }

        for(int i=startIndex;i<=9-(k-path.size())+1;i++){//剪枝优化
            sum+=i;//压入一个i，和就加上i
            path.push_back(i);
            backtracking(k,n,sum,i+1);
            sum-=i;//回溯，减去i
            path.pop_back();
        }
        return;


    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear();
        path.clear();
        backtracking(k,n,0,1);
        return result;
    }
};
```



## 1、电话号码的字母组合

#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

建立数字与字母的映射

注意字符串数字到数字的转换

```c++
class Solution {
private:
    const string lettermap[10]{
        "",//0
        "",//1
        "abc",//2
        "def",//3
        "ghi",//4
        "jkl",//5
        "mno",//6
        "pqrs",//7
        "tuv",//8
        "wxyz"//9
    };
public:
    vector<string> result;
    string s;
    void backtracking(string& digits,int index){
        if(index==digits.size()){
            result.push_back(s);
            return;
        }
        int digit=digits[index]-'0';
        string letter=lettermap[digit];
        for(int i=0;i<letter.size();i++){
            s += letter[i];
            backtracking(digits,index+1);
            s.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        result.clear();
        s.clear();
        if(digits.size()==0) return result;
        backtracking(digits,0);
        return result;
    }
};
```

