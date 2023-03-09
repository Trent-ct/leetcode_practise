# DAY9

## 1、实现strStr()

#####  **28.实现strStr **     https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/

找出字符串中第一个匹配项的下标

采用KMP算法

求取needle的前缀表

当遍历至needle中与haystack不一样的字符时，needle依据前缀表前一位，跳转到最长相同前后缀的后一位处重新开始字符匹配

相当于在字符不匹配处，needle中前面的最长相同前后缀的前缀已由后缀进行匹配过了。

```c++
class Solution {
public:
    void getNext(int* next,const string& s){
        int i=0;
        next[0]=0;
        for(int j=1;j<s.size();j++){
            while(i>0&&s[i]!=s[j]) i=next[i-1];
            if(s[i]==s[j]) i++;
            next[j]=i;
        }
    }
    int strStr(string haystack, string needle) {
        if(needle.size()==0) return 0;
        int next[needle.size()];
        getNext(next,needle);
        int j=0;
        for(int i=0;i<haystack.size();i++){
            while(j>0&&needle[j]!=haystack[i]){
                j=next[j-1];
            }
            if(needle[j]==haystack[i]){
                j++;
            }
            if(j==needle.size()){
                return (i-needle.size()+1);
            }
        }
        return -1;
    }
};
```



## 2、重复的子字符串

#####  459.重复的子字符串     https://leetcode.cn/problems/repeated-substring-pattern/

判断一个字符串是否由一个子串重复构成

核心在于求取最长相等前后缀

然后由字符串减去最长相等前后缀长度就可以得到目标子串

若子串长度能够被字符串长度整除，那么就表示字符串由该子串重复构成



```c++
//使用KMP方法
class Solution {
public:
    void getNext(int* next,const string& s){//求取前缀表
        int i=0;//i为慢指针，处于前缀处
        next[0]=0;
        for(int j=1;j<s.size();j++){//j为快指针
            while(i>0&&s[i]!=s[j]) i=next[i-1];
            if(s[i]==s[j]) i++;
            next[j]=i;
        }
    }
    bool repeatedSubstringPattern(string s) {
        if(s.size()==0) return false;//判断字符串是否为空
        int next[s.size()];
        getNext(next,s);//获取前缀表
        int len=s.size();
        if(next[len-1]!=0&&len%(len-next[len-1])==0) return true;
        return false;
    }
};
```



若由同一子串重复构成，那么在字符串末尾处衔接一个相同字符串，新字符串中一定包含了原来的字符串

移除匹配就是拼接两个旧字符串，然后去除头尾字符，遍历新字符串若存在旧字符串则表明旧字符串是由子串重复构成的。

```c++
//使用移除匹配的方法
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        string t = s + s;
        t.erase(t.begin()); t.erase(t.end() - 1); // 掐头去尾
        if (t.find(s) != std::string::npos) return true; // r
        return false;
    }
};
```

