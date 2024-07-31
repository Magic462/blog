---
title: 移动应用开发实验室大一下考核题分析
date: 2024-04-21 17:20:09
updated: 2024-04-22 13:44:36
categories: 算法
tags: [算法, 数据结构]
---
# 力扣238除自身以外数组的乘积

给你一个整数数组 nums，返回 数组 answer ，其中 answer[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积 。

题目数据 保证 数组 nums之中任意元素的全部前缀元素和后缀的乘积都在  32 位 整数范围内。

请 不要使用除法，且在 O(n) 时间复杂度内完成此题。

 

示例 1:

输入: nums = `[1,2,3,4]`
输出: `[24,12,8,6]`


示例 2:

输入: nums = `[-1,1,0,-3,3]`
输出: `[0,0,9,0,0]`
 

提示：

2 <= nums.length <= 105
-30 <= nums[i] <= 30
保证 数组 nums之中任意元素的全部前缀元素和后缀的乘积都在  32 位 整数范围内

## 题目分析

输出一个数组，数组的第i位是除第i个数字的其他元素之积，不能使用除法

## 解题思路

利用前缀积和后缀积的思想，数组的每一位都等于它的前缀积×后缀积
注意：这里的前缀和后缀与正常不同，不能包含第i位

## 代码实现

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        
        vector<int> a(nums.size(),0),b(nums.size(),0);
        vector<int> ans(nums.size(),1);
        a[0]=1;
        b[nums.size()-1]=1;
        for(int i=1,j=nums.size()-2;j>=0;i++,j--){
           a[i]=a[i-1]*nums[i-1];
           b[j]=b[j+1]*nums[j+1];
        }
        for(int i=0;i<nums.size();i++){
            ans[i]=a[i]*b[i];
        }
        return ans;
    }
};
```

# 力扣394字符串解码

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

 

示例 1：

输入：s = "`3[a]2[bc]`"
输出："aaabcbc"


示例 2：

输入：s = "`3[a2[c]]`"
输出："accaccacc"


示例 3：

输入：s = "`2[abc]3[cd]ef`"
输出："abcabccdcdcdef"


示例 4：

输入：s = "`abc3[cd]xyz`"
输出："abccdcdcdxyz"
 

提示：

1 <= s.length <= 30
s 由小写英文字母、数字和方括号 '[]' 组成
s 保证是一个 有效 的输入。
s 中所有整数的取值范围为 [1, 300] 

## 题目分析

有关括号的匹配的字符串问题

## 解题思路

当我们遇到括号的匹配时，首先想到的就是用栈来解决
设置两个栈nums,strs,分别来存放字符串中的数字与字符串，定义一个字符串res用于输出1.遇到数字用num记录，遇到字符串用res记录
2.遇到左括号时，把数字和字符串分别压入栈，再重置num为0，res为空
3.遇到右括号时，开始解码，取num的栈顶作为字符串重复的次数，重复将res加入栈内后，若栈顶还是字母，就会直接加到res之后，因为它们是同一级的运算，若是左括号，res会被压入str，作为上一层的运算

## 代码实现

```cpp
class Solution {
public:
    string decodeString(string s) {
        string res = "";
        stack <int> nums;
        stack <string> strs;
        int num = 0;
        int len = s.size();
        for(int i = 0; i < len; ++ i)
        {
            if(s[i] >= '0' && s[i] <= '9')
            {
                num = num * 10 + s[i] - '0';
            }
            else if((s[i] >= 'a' && s[i] <= 'z') ||(s[i] >= 'A' && s[i] <= 'Z'))
            {
                res = res + s[i];
            }
            else if(s[i] == '[') //将‘[’前的数字压入nums栈内， 字母字符串压入strs栈内
            {
                nums.push(num);
                num = 0;
                strs.push(res); 
                res = "";
            }
            else //遇到‘]’时，操作与之相配的‘[’之间的字符，使用分配律
            {
                int times = nums.top();
                nums.pop();
                for(int j = 0; j < times; ++ j)
                    strs.top() += res;
                res = strs.top(); //之后若还是字母，就会直接加到res之后，因为它们是同一级的运算
                                  //若是左括号，res会被压入strs栈，作为上一层的运算
                strs.pop();
            }
        }
        return res;
    }
};
```

# 力扣23合并k个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

示例 1：

输入：lists = `[[1,4,5],[1,3,4],[2,6]]`
输出：`[1,1,2,3,4,4,5,6]`
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6


示例 2：

输入：lists = `[]`
输出：`[]`
示例 3：

输入：lists = `[[]]`
输出：`[]`

## 题目分析

与合并2个有序链表类似

## 解题思路

先合并前两个链表，再不断将新链表与下一个链表合并

## 代码实现

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode *a, ListNode *b) {
        if ((!a) || (!b)) return a ? a : b;
        ListNode head, *tail = &head, *aPtr = a, *bPtr = b;
        while (aPtr && bPtr) {
            if (aPtr->val < bPtr->val) {
                tail->next = aPtr; aPtr = aPtr->next;
            } else {
                tail->next = bPtr; bPtr = bPtr->next;
            }
            tail = tail->next;
        }
        tail->next = (aPtr ? aPtr : bPtr);
        return head.next;
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *ans = nullptr;
        for (int i = 0; i < lists.size(); ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }
};
```


# 力扣232用栈实现队列

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：

实现 MyQueue 类：

void push(int x) 将元素 x 推到队列的末尾
int pop() 从队列的开头移除并返回元素
int peek() 返回队列开头的元素
boolean empty() 如果队列为空，返回 true ；否则，返回 false
说明：

你 只能 使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
 

示例 1：

输入：
`["MyQueue", "push", "push", "peek", "pop", "empty"]`
`[[], [1], [2], [], [], []]`

输出：
`[null, null, null, 1, 1, false]`

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
 

提示：

1 <= x <= 9
最多调用 100 次 push、pop、peek 和 empty
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）

## 题目分析

用栈实现队列

## 解题思路

C语言
myQueueCreate
功能：创建一个新的队列，并初始化栈顶指针。
myQueuePush
功能：将一个元素推入队列。
myQueuePop
功能：从队列中弹出一个元素。
如果 stkOut 是空的，它会从 stkIn 中取出所有元素并放入 stkOut 中，然后弹出 stkOut 的顶部元素。这是为了确保弹出的元素是队列中最先进入的元素。
myQueuePeek
功能：查看队列的顶部元素但不弹出。
myQueueEmpty
功能：检查队列是否为空。
myQueueFree
功能：释放队列。

## 代码分析

```c
typedef struct {
    int stkInTop,stkOutTop;
    int stkIn[100],stkOut[100];
} MyQueue;


MyQueue* myQueueCreate() {
    MyQueue* queue = (MyQueue*)malloc(sizeof(MyQueue));
    queue->stkInTop = 0;
    queue->stkOutTop = 0;
    return queue;
}

void myQueuePush(MyQueue* obj, int x) {
    obj->stkIn[(obj->stkInTop)++] = x;
}

int myQueuePop(MyQueue* obj) {
    int inTop = obj->stkInTop;
    int outTop = obj->stkOutTop;
    if(outTop == 0){
        while(inTop){
            obj->stkOut[outTop++] = obj->stkIn[--inTop];
        }
    }
    int res = obj->stkOut[--outTop];
    while(outTop){
        obj->stkIn[inTop++] = obj->stkOut[--outTop];
    }
    obj->stkInTop = inTop;
    obj->stkOutTop = outTop;
    return res;
}

int myQueuePeek(MyQueue* obj) {
    return obj->stkIn[0];
}

bool myQueueEmpty(MyQueue* obj) {
    return obj->stkInTop == 0 && obj->stkOutTop == 0;
}

void myQueueFree(MyQueue* obj) {
    obj->stkInTop = 0;
    obj->stkOutTop = 0;
}

```

# 力扣61旋转链表

给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。

 

示例 1：


输入：head = `[1,2,3,4,5]`, k = 2
输出：`[4,5,1,2,3]`


示例 2：


输入：head = `[0,1,2]`, k = 4
输出：`[2,0,1]`
 

提示：

链表中节点的数目在范围 [0, 500] 内
-100 <= Node.val <= 100
0 <= k <= 2 * 109

## 题目分析

将旋转后的链表重新排序

## 解题思路

将链表存到数组中，再用三个reverse反转数组，再存入数组中
三个反转：先全部反转，再反转前k个，最后反转后k个

## 代码实现

```cpp
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
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head){
            return head;
        }
        ListNode*p=head;
        vector<int>arr;
        int n = 1;
        ListNode* iter = head;
        while (iter->next != nullptr) {
            iter = iter->next;
            n++;
        }
        int add = k % n;
        if (add == n) {
            return head;
        }
        while(p->next!=nullptr){
            arr.push_back(p->val);
             p=p->next;
        }
        arr.push_back(p->val);
        p=head;
        reverse(arr.begin(),arr.end());
        reverse(arr.begin(),arr.begin()+add);
        reverse(arr.begin()+add,arr.end());
        for(int i=0;i<arr.size();i++){
            p->val=arr[i];
            p=p->next;
        }
        return head;
    }
};
```

# 力扣392判断子序列

给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

进阶：

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

致谢：

特别感谢 @pbrother 添加此问题并且创建所有测试用例。

 

示例 1：

输入：s = "abc", t = "ahbgdc"
输出：true

示例 2：

输入：s = "axc", t = "ahbgdc"
输出：false
 

提示：

0 <= s.length <= 100
0 <= t.length <= 10^4
两个字符串都只由小写字符组成
。
## 题目分析

判断s是否是t的子序列，其中可以有别的字符，但不能改变相对位置

## 解题思路

利用快慢指针，慢指针位于s，快指针位于t，只有快慢指针指向的字符相等时慢指针移动，快指针持续移动，当慢指针遍历完时（也就是快指针已经将慢指针指向的字符都匹配上）返回true

## 代码实现

```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int slowindex,fastindex;
        if(s.size()==0)return true;
        for(fastindex=0,slowindex=0;fastindex<t.size();fastindex++)
        {
            if(s[slowindex]==t[fastindex])
            {
                if(++slowindex==s.size())
                return true;
            }
        }
        return false;
    }
};
```
