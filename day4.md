# DAY4

## 1、两两交换链表中的节点

##### 24.两两交换链表中的节点     https://leetcode.cn/problems/swap-nodes-in-pairs/

增加虚拟头节点模拟交换过程即可，注意循环判断的条件，可能存在野指针

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
    ListNode* swapPairs(ListNode* head) {
        ListNode* dhead=new ListNode(0);
        dhead->next=head; //虚拟头节点
        ListNode* tmp; //存储后一个
        ListNode* cur=dhead;//当前节点
        ListNode* pre;//前一个
        while(cur->next!=nullptr&&cur->next->next!=nullptr){
            pre=cur->next;//存储后一节点
            tmp=cur->next->next;//存储后二节点
            cur->next=tmp;//开始连接交换
            pre->next=tmp->next;
            tmp->next=pre;
            cur=cur->next->next;
        }
        head=dhead->next;
        return head;
    }
};
```



## 2、删除节点

#####  19.删除链表的倒数第N个节点    https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

这是看到题目就想到的思路，整体就是通过遍历链表得到长度后再进行定位

而代码随想录中运动快慢指针定位的方法要更为快捷，快指针运动到N+1的位置后，慢指针再一起运动，直到快指针为空

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dhead=new ListNode(0);
        dhead->next=head;
        ListNode* cur=dhead;
        int _size=0;
        while(cur->next){ //统计链表长度
            cur=cur->next;
            _size++;
        }
        int count=_size-n;
        cur=dhead;
        while(count--){//定位到需要删除节点的前一个
            cur=cur->next;
        }
        ListNode* tmp=cur->next->next;//进行删除操作
        ListNode* tmp1=cur->next;
        cur->next=tmp;
        delete tmp1;//释放内存
        return dhead->next;//有可能原本的头节点已被删除
    }
};
```

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dhead=new ListNode(0);
        dhead->next=head;
        ListNode* fast=dhead;
        ListNode* slow=dhead;
        n=n+1;
        while(n--){//快指针运动n+1
            fast=fast->next;
        }
        while(fast){//快慢一起运动直到快指针为空
            fast=fast->next;
            slow=slow->next;
        }
        ListNode* tmp=slow->next;//用于释放内存
        slow->next=slow->next->next;//删除节点
        delete tmp;
        return dhead->next;
    }
};
```



## 3、链表相交

#####  面试题02.07.链表相交  https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/

看到的第一思路就是暴力两层循环求解，但显然不是好的解

求相交节点，核心思想为链表末端对齐，题目中链表为尾部相交，所以长链表头部多出的部分不可能发生相交，从末端对齐处开始比较两条链表的节点是否一致

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA=dheadA;
        ListNode* curB=dheadB;
        int lenA=0,lenB=0;
        while(curA){//求链表A长度
            lenA++;
            curA=curA->next;
        }
        while(curB){//求链表B长度
            lenB++;
            curB=curB->next;
        }
        curA=headA;
        curB=headB;
        if(lenB>lenA){//使链表A为长链表
            swap(lenA,lenB);
            swap(curA,curB);
        }
        int gap=lenA-lenB;
        while(gap--){//两条链表末端对齐
            curA=curA->next;
        }
        while(curA){//寻找相交节点
            if(curA==curB){
                return curA;
            }
            curA=curA->next;
            curB=curB->next;
        }
        return NULL;
    }
};
```

## 4、环形链表

#####  142.环形链表Ⅱ  https://leetcode.cn/problems/linked-list-cycle-ii/

该问题可以分为两部分，一是判断链表中是否存在环，二是寻找环的入口

判断是否存在环：使用速度相差为1的快慢指针遍历链表，如果出现快指针与慢指针相等的情况，则表明链表中存在环。

寻找环的入口：重新定义两个指针，一个从头节点出发，一个从快慢指针相遇点出发，且两者运动速度相同，两个指针最后相遇的地方就是环的入口

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast=head;
        ListNode* slow=head;
        ListNode* index1=head;//寻找入口时所用头节点
        ListNode* index2;//记录快慢指针相遇时的节点
        while(fast!=nullptr&&fast->next!=nullptr){
            fast=fast->next->next;
            slow=slow->next;
            if(fast==slow){
                index2=fast;
                while(index1!=index2){
                    index1=index1->next;
                    index2=index2->next;
                }
                return index1;
            }
        }
        return NULL;
    }
};
```

