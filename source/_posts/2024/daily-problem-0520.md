---
title: 移动应用开发实验室大一下考核题分析
date: 2024-05-20 17:20:29
updated: 2024-05-21 13:14:09
categories: 算法
tags: [算法, 数据结构]
---
# 力扣LCR024
给定单链表的头节点 head ，请反转链表，并返回反转后的链表的头节点。

示例 1：

输入：head = `[1,2,3,4,5]`

输出：`[5,4,3,2,1]`


示例 2：

输入：head = `[1,2]`

输出：`[2,1]`


示例 3：

输入：head = `[]`

输出：`[]`
 

提示：
链表中节点的数目范围是 [0, 5000]
-5000 <= Node.val <= 5000

## 题目分析

题目很容易理解，反转链表即可

## 解题思路

利用循环
首先设置一个空指针充当反转后的尾节点后的空值，然后使头节点指向空值，再依次移动指针，实现反转，注意，反转时要先设置一个指针来保存反转的下一个节点，否则会丢失此节点

## 优化

利用递归
首先判断是否只有一个节点
然后利用reverse的功能将head后的链表都反转，注意将head下一个节点指向的空指针指向head,再将head指向NULL

## 代码实现

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode*pre=NULL;
        ListNode*cur=head;
        while(cur!=NULL){
            ListNode*tmp=cur->next;
            cur->next=pre;
            pre=cur;
            cur=tmp;
        }
        return pre;
    }
};
```

# 力扣LCR136删除链表节点

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

示例 1:

输入: head = `[4,5,1,9]`, val = 5

输出: `[4,1,9]`

解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.


示例 2:

输入: head = `[4,5,1,9]`, val = 1

输出: `[4,5,9]`

解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
 

说明：

题目保证链表中节点的值互不相同
若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点

## 题目分析

删除链表节点

## 解题思路

定位要被删除节点的前一个节点

## 代码实现

```cpp
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        if(head->val==val){
            return head->next;
        }
        ListNode*cur=head->next;
        ListNode*pre=head;
        while(cur->val!=val&&cur!=NULL){
            pre=cur;
            cur=cur->next;
        }
        pre->next=cur->next;
        return head;
    }
};
```

# 力扣142环形链表

给定一个链表的头节点 head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

示例 1：

输入：head = `[3,2,0,-4]`, pos = 1

输出：返回索引为 1 的链表节点

解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = `[1,2]`, pos = 0

输出：返回索引为 0 的链表节点

解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：

输入：head = `[1]`, pos = -1

输出：返回 null

解释：链表中没有环。

## 题目分析

如果链表是环形链表，返回链表成环的入口，如果链表不是链表返回NULL

## 解题思路

利用快慢指针，假如快指针比慢指针多走一个节点（其实只要比慢指针快即可），如果链表为环形，快慢指针一定会相遇
1找到相遇的节点
2我们会发现如果相遇节点和头节点同时走，相遇的节点就是环形的入口
原理：设置3个变量
x为头节点到链表入口的距离
y为入口到相遇节点的距离
z为入口到相遇节点的另一半距离
快指针比慢指针多走了n圈，所以慢指针路程为x+y,快指针为x+y+n(y+z),而快指针行走的路程是慢指针的2倍，可列出等式
整理得x=(n-1)(y+z)+z,当n=1时，x=z，即头节点到链表入口的距离等于入口到相遇节点的另一半距离

## 代码实现

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode*slow=head;
        ListNode*fast=head;
        ListNode*pre=head;
        while(fast!=NULL&&fast->next!=NULL){
            slow=slow->next;
            fast=fast->next->next;
            if(slow==fast){
                while(pre!=fast){
                    pre=pre->next;
                    fast=fast->next;
                }
                return pre;
            }
        }
        return NULL;
    }
};

```

# 力扣160相交链表

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：

题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

示例 1：

输入：intersectVal = 8, listA = `[4,1,8,4,5]`, listB = `[5,0,1,8,4,5]`, skipA = 2, skipB = 3

输出：Intersected at ‘8’

解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。


示例 2：

输入：intersectVal = 2, listA = `[0,9,1,2,4]`, listB = `[3,2,4]`, skipA = 3, skipB = 1
输出：Intersected at ‘2’
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 `[0,9,1,2,4]`，链表 B 为 `[3,2,4]`。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
示例 3：

输入：intersectVal = 0, listA = `[2,6,4]`, listB = `[1,5]`, skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 `[2,6,4]`，链表 B 为 `[1,5]`。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。

提示：

listA 中节点数目为 m
listB 中节点数目为 n
0 <= m, n <= 3 * 104
1 <= Node.val <= 105
0 <= skipA <= m
0 <= skipB <= n
如果 listA 和 listB 没有交点，intersectVal 为 0
如果 listA 和 listB 有交点，intersectVal == listA[skipA + 1] == listB[skipB + 1]

进阶：你能否设计一个时间复杂度 O(n) 、仅用 O(1) 内存的解决方案？

## 题目分析

找到链表指针相等的节点并返回数值
注意题目想要的是节点的指针对齐而不是对应的数值相等

## 解题思路

先找到链表从后对齐的点，再寻找相等的节点，返回节点
1计算两个链表的长度的差值，再利用while就可以定位链表长度相等的地方
2再遍历链表，遇到相等就返回

代码实现

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int len1=0,len2=0;
        ListNode*curA=headA;
        ListNode*curB=headB;
        while(curA!=NULL){
            len1++;
            curA=curA->next;
        }
        while(curB!=NULL){
            len2++;
            curB=curB->next;
        }
        curA=headA;
        curB=headB;
        if(len2>len1){
            swap(curA,curB);
            swap(len1,len2);
        }
        int c=len1-len2;
        while(c--){
            curA=curA->next;
        }
        while(curA!=NULL){
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

# 力扣234回文链表

给你一个单链表的头节点 head ，请你判断该链表是否为
回文链表
。如果是，返回 true ；否则，返回 false 。

 

示例 1：

输入：head = `[1,2,2,1]`

输出：true


示例 2：

输入：head = `[1,2]`

输出：false
 
提示：

链表中节点数目在范围[1, 105] 内
0 <= Node.val <= 9
 
## 题目分析

判断一个链表是否为回文链表

## 解题思路

利用快慢指针，快指针一次走两步，慢指针走一步，则当快指针为空时慢指针走到了中间节点，然后反转后半部分链表，再检查是否与前半部分链表匹配，最后将反转的链表反转回来

## 代码实现

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == nullptr){
            return true;
        }
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast->next != nullptr && fast->next->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
        }
        ListNode* prev = nullptr;
        ListNode* cur = slow->next;
        while(cur != nullptr){
            ListNode* next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        ListNode* p1 = head;
        ListNode* p2 = prev;
        bool result = true;
        while(result && p2 != nullptr){
            if(p1->val != p2->val){
                result = false;
                break;
            }
            p1 = p1->next;
            p2 = p2->next;
        }
        ListNode* prev2 = nullptr;
        cur = prev;
        while(cur != nullptr){
            ListNode* next = cur->next;
            cur->next = prev2;
            prev2 = cur;
            cur = next;
        }
        slow->next = prev2;
        return result;
    }
};
```

# 力扣203移除链表
给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
 

示例 1：


输入：head = `[1,2,6,3,4,5,6]`, val = 6
输出：`[1,2,3,4,5]`
示例 2：

输入：head = [], val = 1
输出：[]
示例 3：

输入：head = `[7,7,7,7]`, val = 7
输出：[]
 

提示：

列表中的节点数目在范围 [0, 104] 内
1 <= Node.val <= 50
0 <= val <= 50

## 题目分析

删除链表中所有等于目标值的结点

## 解题思路

for循环遍历链表，找到目标值时移除链表节点，注意跳出循环条件

## 优化

因为头节点不为空，所以对于头结点的操作与其他节点不同
这时我们可以设置一个空的节点指向头节点，也就是虚拟头节点

## 代码实现

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
       ListNode*dummyhead=new ListNode(0);
       dummyhead->next=head;
       ListNode*cur=dummyhead;
       while(cur->next!=NULL)
       {
        if(cur->next->val==val)
        {
            ListNode*tmp=cur->next;
            cur->next=cur->next->next;
            delete tmp;
        }else{
            cur=cur->next;
        }
       }
       return dummyhead->next;
    }
};
```

# 力扣876链表的中间结点
给你单链表的头结点 head ，请你找出并返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

 

示例 1：

输入：head = `[1,2,3,4,5]`

输出：`[3,4,5]`

解释：链表只有一个中间结点，值为 3 。


示例 2：

输入：head = `[1,2,3,4,5,6]`

输出：`[4,5,6]`

解释：该链表有两个中间结点，值分别为 3 和 4 ，返回第二个结点。

 ## 题目分析

 若有一个中间结点则返回中间结点，若有两个中间结点，则返回第二个中间结点

 ## 解题思路

 先遍历链表求出链表长度，长度/2就可到达中间结点，注意偶数结点返回的是中间的第二个结点

 ## 代码实现
 

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode*cur=head;
        int len=0;
        while(cur!=NULL){
            len++;
            cur=cur->next;
        }
        ListNode*pre=head;
        for(int i=len/2;i>0;i--){
                pre=pre->next;
            }
            return pre;
    }
};
```

 # 力扣21合并两个有序链表

 将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。


示例 1：

输入：l1 = `[1,2,4]`, l2 = `[1,3,4]`

输出：`[1,1,2,3,4,4]`


示例 2：

输入：l1 =`[]`, l2 = `[]`

输出：`[]`


示例 3：

输入：l1 = `[]`, l2 = `[0]`

输出：`[0]`

提示：

两个链表的节点数目范围是 [0, 50]
-100 <= Node.val <= 100
l1 和 l2 均按 非递减顺序 排列

## 题目分析

将两个有序链表合并为一个有序链表

## 解题思路

另外定义新链表，依次比较两个链表的值再存入
或直接使用递归将其拼接
问题：使用递归将返回l1还是l2呢？
随着递归使用，当有一个链表为空时，返回另外一个
注意：最后的判断条件不可为l1->val>=l2->next，因为必须要进入if else 语句，否则没有返回值，编译报错
如果新建链表则没有此问题

## 代码实现

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1==NULL)
        {
            return l2;
        }else if(l2==NULL)
        {
            return l1;
        }else if(l1->val<l2->val)
        {
            l1->next=mergeTwoLists(l1->next,l2);
            return l1;
        }else //不能写l1->next>=l2->next
        {
            l2->next=mergeTwoLists(l1,l2->next);
            return l2;
        }
    }
};

```