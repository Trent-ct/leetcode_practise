# DAY10

## 1、栈实现队列

#####  **232.用栈实现队列**     https://leetcode.cn/problems/implement-queue-using-stacks/

利用两个栈来实现队列的功能，入栈导入到出栈后，其元素弹出的顺序就和队列弹出的顺序一致了

注意入栈导入到出栈时出栈要为空，否则新的元素就插入到旧元素的前面了，违反了先入先出的规则

```c++
class MyQueue {
public:
    stack<int> stack_in;//入栈
    stack<int> stack_out;//出栈

    MyQueue() {//缺省构造函数

    }
    
    void push(int x) {
        stack_in.push(x);//插入到入栈中
    }
    
    int pop() {
        if(stack_out.empty()){
            //每次将入栈全部导入到出栈后，一定要等出栈pop至空时才能再次导入，否则顺序就乱了
            while(!stack_in.empty()){//出栈导入至入栈
                stack_out.push(stack_in.top());
                stack_in.pop();
            }
        }
        int result=stack_out.top();
        stack_out.pop();
        return result;
    }
    
    int peek() {
        //直接取TOP时出栈可能为空，所以增加一步将入栈导入到出栈的过程
        if(stack_out.empty()){
            while(!stack_in.empty()){
                stack_out.push(stack_in.top());
                stack_in.pop();
            }
        }
        int res=stack_out.top();
        return res;
        //也可以利用对象中已经写好的pop函数来实现peek功能
        //int res = this->pop(); // 直接使用已有的pop函数
        //stOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        //return res;
    }
    
    bool empty() {//出栈入栈皆为空则表明队列为空
        if(stack_in.empty()&&stack_out.empty()) return true;
        return false;
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```



## 2、队列实现栈

#####  225.用队列实现栈     https://leetcode.cn/problems/implement-stack-using-queues/

使用一个队列实现栈，不断地将队列的头弹出然后插入至末尾，直到队列的末尾元素移动至队列的头部，然后再弹出队列的头部元素，就实现了栈的pop功能

```c++
class MyStack {
public:
    queue<int> que;
    MyStack() {

    }
    
    void push(int x) {
        que.push(x);
    }
    
    int pop() {
        //将队列的头弹出并插入至队列尾，直到原来的队列尾到达队列头
        for(int i=0;i<que.size()-1;i++){
            int tmp=que.front();
            que.pop();
            que.push(tmp);
        }
        //取队列头的值后弹出队列头，实现pop功能
        int result=que.front();
        que.pop();
        return result;
    }
    
    int top() {
        //调用已经实现的pop函数，取到队列末尾值后要再次将值插入至末尾
        int res=this->pop();
        que.push(res);
        return res;
    }
    
    bool empty() {//队列为空则栈为空
        return que.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```


