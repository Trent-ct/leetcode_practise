# DAY8

## 1、反转字符串

#####  **344.反转字符串 **     https://leetcode.cn/problems/reverse-string/submissions/

双指针分别指向字符串头尾，边交换边向中间移动

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i,j;
        for(i=0,j=s.size()-1;i<j;i++,j--){
            swap(s[i],s[j]);
        }
    }
};
```



## 2、反转字符串Ⅱ

#####  541.反转字符串Ⅱ     https://leetcode.cn/problems/reverse-string-ii/

前2k个字符都按要求正常进行反转，最后剩余的字符串由于存在不满2k的可能性，进行分类判断后再反转

```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        int size=s.size();
        int res=size%(2*k);
        int count=size/(2*k);
        for(int i=0;i<count;i++){
            for(int left=i*2*k,right=i*2*k+k-1;left<right;left++,right--){
                swap(s[left],s[right]);
            }
        }
        if(res<k){
            for(int left=size-res,right=size-1;left<right;left++,right--){
                swap(s[left],s[right]);
            }
        }else if(res>=k){
            for(int left=count*2*k,right=count*2*k+k-1;left<right;left++,right--){
                swap(s[left],s[right]);
            }
        }
        return s;
    }
};
```

代码随想录中的写法要更为简洁，实际上字符串末尾数量大于k小于2k时与末尾数量等于2k的情况下的操作是一致的，只有当末尾数量小于k时操作的字符串数量才会发生变化。

```c++
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += (2 * k)) {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k );
            } else {
                // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
                reverse(s.begin() + i, s.end());
            }
        }
        return s;
    }
}
```



## 3、替换空格

#####   剑指Offer 05.替换空格  https://leetcode.cn/problems/ti-huan-kong-ge-lcof/

由于%20是三位，而空格只占一位，所以进行替换时需要先将字符串扩充，每有一个空格需要扩充两位

进行空格替换时使用双指针从后向前遍历，保证旧指针在新指针之前，

```c++
class Solution {
public:
    string replaceSpace(string s) {
        int num=0;//记录空格数量
        int size_old=s.size();
        for(int i=0;i<s.size();i++){
            if(s[i]==' ') num++;
        }
        //扩充字符数组
        s.resize(s.size()+2*num);
        int size_new=s.size();
        for(int i=size_old-1,j=size_new-1;i<j;i--,j--){
            if(s[i]!=' '){//不是空格，直接替换
                s[j]=s[i];
            }else if(s[i]==' '){//是空格，替换为%20
                s[j]='0';
                s[j-1]='2';
                s[j-2]='%';
                j=j-2;
            }
        }
        return s;

    }
};
```

## 4、翻转字符串里的单词

#####  151.翻转字符串里的单词  https://leetcode.cn/problems/reverse-words-in-a-string/

使用快慢双指针移除字符串中的空格，然后反转整个字符串，再对字符串中的单词进行反转

```c++
class Solution {
public:
    void reverse(string& s,int begin,int end){//反转字符串，区间为左闭右闭
        for(int left=begin,right=end;left<right;left++,right--){
            swap(s[left],s[right]);
        }
    }
    string reverseWords(string s) {
        //移除多余空格
        int slow=0;
        int fast=0;
        for(slow=0,fast=0;fast<s.size();++fast){
            if(s[fast]!=' '){//当快指针找到不是空格的字母时
                if(slow!=0) s[slow++]=' ';//空出一个空格开始新的单词
                while(fast<s.size()&&s[fast]!=' '){//将新的单词完整移植
                    s[slow]=s[fast];
                    slow++;
                    fast++;
                }
            }
        }
        //反转字符串
        s.resize(slow);
        reverse(s,0,s.size()-1);
        //反转单词
        int tmp=0;
        for(int i=0;i<=s.size();++i){
            if(s[i]==' '||i==s.size()){
                reverse(s,tmp,i-1);
                tmp=i+1;
            }
        }
        return s;
    }
};
```

## 5、左旋转字符串

##### 剑指Offer58-II.左旋转字符串 https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

先反转需要左旋转位置处左边和右边的字符串，然后将整个字符串再次反转。

```c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(),s.begin()+n);//左闭右开
        reverse(s.begin()+n,s.end());
        reverse(s.begin(),s.end());
        return s;
    }
};
```

