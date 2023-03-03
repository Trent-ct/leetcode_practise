# DAY3

## 1、移除链表元素

##### **203.移除链表元素**     https://leetcode.cn/problems/remove-linked-list-elements/

将需要移除的元素的前一节点指针直接指向后一节点即可完成元素移除（注意被移除元素占用内存的释放）。

为使移除过程更加统一，设定虚拟头节点，使得头节点的删除与后续节点的删除一致。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dhead=new ListNode(0);//定义虚拟头节点
        dhead->next=head;//虚拟头节点指向真实头节点
        ListNode* cur=dhead;//标记当前节点
        while(cur->next!=NULL){
            if(cur->next->val==val){//判断是否为需要删除的元素
                ListNode* tmp=cur->next;//存储被删除元素
                cur->next=cur->next->next;//删除元素
                delete tmp;//释放内存
            }else{
                cur=cur->next;
            }
        } 
        head=dhead->next;//设定新的头节点
        delete dhead;
        return head;
    }
};
```



## 2、设计链表

#####  707.设计链表    https://leetcode.cn/problems/design-linked-list/

设计链表主要涉及初始化、查询、插入头节点、插入尾节点、插入任意索引节点、删除任意索引节点

注意构造函数以及new的用法

实现的功能互相之间有一定联系

```c++
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```



```c++
class MyLinkedList {
public:
    struct ListNode {
      int val;
      ListNode *next;
      ListNode(int x) : val(x), next(nullptr) {}
    };
    //初始化
    MyLinkedList() {
        dhead=new ListNode(0);
        _size=0;
    }
    //查询
    int get(int index) {
        if(index<0||index>(_size-1)){
            return -1;
        }
        ListNode* cur=dhead->next;
        while(index--){
            cur=cur->next;
        }
        return cur->val;
    }
    //插入头节点
    void addAtHead(int val) {
        ListNode* insert_h=new ListNode(val);
        insert_h->next=dhead->next;
        dhead->next=insert_h;
        _size++;
    }
    //插入尾节点
    void addAtTail(int val) {
        ListNode* insert_t=new ListNode(val);
        ListNode* cur=dhead;
        int count=_size;
        while(count--){
            cur=cur->next;
        }
        cur->next=insert_t;
        _size++;
    }
    //任意位置插入节点
    void addAtIndex(int index, int val) {
        if(index>_size) return;
        if(index<0) index=0;
        ListNode* insert_m=new ListNode(val);
        ListNode* cur=dhead;
        int count=index;
        while(count--){
            cur=cur->next;
        }
        insert_m->next=cur->next;
        cur->next=insert_m;
        _size++;
    }
    //删除任意节点
    void deleteAtIndex(int index) {
        if(index<0||index>_size-1) return;
        ListNode* cur=dhead;
        ListNode* tmp;
        int count=index;
        while(count--){
            cur=cur->next;
        }
        tmp=cur->next;
        cur->next=cur->next->next;
        delete tmp;
        _size--;


    }
    //私有成员，定义初始化的两个变量
    private:
      int _size;
      ListNode* dhead;
};
```



## 3、反转链表

#####  206.反转链表  https://leetcode.cn/problems/reverse-linked-list/

反转链表的核心思想是转换链表内指针的指向，进而实现头变尾、尾变头

循环条件选用cur，随着循环的进行，最后cur会为空

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* tmp; //用于存储cur的下一个节点
        ListNode* pre=nullptr;//用于cur指向上一节点
        ListNode* cur=head;//当前所在节点
        while(cur){
            tmp=cur->next;
            cur->next=pre;
            pre=cur;
            cur=tmp;
        }
        head=pre; //反转结束，重新设定头节点
        return head;
    }
};
```

