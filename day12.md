# DAY12

## 1、有效的括号

#####  **20.有效的括号 **     [20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/)

将左括号压入栈中，然后用右括号进行匹配

```c++
class Solution {
public:
    bool isValid(string s) {
        if(s.size()%2!=0) return false;//奇数时括号无法完成匹配
        stack<char> sta;
        for(int i=0;i<s.size();i++){
            if(s[i]=='(') sta.push(')');//左括号时将右括号压入栈中
            else if(s[i]=='[') sta.push(']');
            else if(s[i]=='{') sta.push('}');
            //当栈为空时表示没有对应左括号，不相等表示括号不匹配
            else if(sta.empty()||s[i]!=sta.top()) return false;
            else if(s[i]==sta.top()) sta.pop();//相等时消去括号，弹出
        }
        return sta.empty();
    }
};
```



## 2、删除字符串中相邻重复项

#####  [1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

用栈对字符串中相邻重复项进行删除，有利于删除一个重复项后再次删除重复项，而不需要重新开始一个循环

注意从栈中依序弹出的字符串与原先字符串相比是逆序的

```c++
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> sta;
        //将字符串压入栈中
        for(int i=0;i<s.size();i++){
            //栈不为空且当前字符与栈顶字符相同时，表明需要删除
            if(!sta.empty()&&s[i]==sta.top()){
                sta.pop();//弹出栈顶元素
            }else{
                sta.push(s[i]); //栈为空或当前字符与栈顶字符不相同，压入元素
            }
        }
        int count=0;
        int size=sta.size();
        //取删除重复元素后的字符串
        for(count;count<size;count++){
            s[count]=sta.top();
            sta.pop();
        }
        //栈中输出的字符串与原先字符串顺序不同
        reverse(s.begin(),s.begin()+count);//字符串逆序
        return {s.begin(),s.begin()+count};//返回删除重复元素后的字符串
    }
};
```



## 3、逆波兰表达式求值

#####   [150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)

注意使用stoll函数对字符串中的数字进行转换

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> sta;
        for(int i=0;i<tokens.size();i++){
            //遇到运算符则计算stack里的后两个数
            if(tokens[i]=="+"||tokens[i]=="-"||tokens[i]=="*"||tokens[i]=="/"){
                int num1=sta.top();//运算符的后一位数
                sta.pop();
                int num2=sta.top();//运算符的前一位数
                sta.pop();
                if(tokens[i]=="+") sta.push(num1+num2);
                if(tokens[i]=="-") sta.push(num2-num1);
                if(tokens[i]=="*") sta.push(num1*num2);
                if(tokens[i]=="/") sta.push(num2/num1);
            }else {
                sta.push(stoll(tokens[i]));
            }
        }
        return sta.top();
    }
};
```

