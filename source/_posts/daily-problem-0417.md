---
title: 每日一题-力扣287寻找重复数、力扣155最小栈
date: 2024-04-17 22:53:09
updated: 2024-07-23 17:36:37
categories: 算法
tags: [算法, 数据结构]
---
# 力扣287寻找重复数

给定一个包含 n + 1 个整数的数组 nums ，其数字都在 [1, n] 范围内（包括 1 和 n），可知至少存在一个重复的整数。

假设 nums 只有 一个重复的整数 ，返回 这个重复的数 。

你设计的解决方案必须 不修改 数组 nums 且只用常量级 O(1) 的额外空间。

 

示例 1：

输入：`nums = [1,3,4,2,2]`
输出：`2`


示例 2：

输入：`nums = [3,1,3,4,2]`
输出：`3`


示例 3 :

输入：`nums = [3,3,3,3,3]`
输出：`3`


## 题目分析
一共有n+1个整数，但只有n个数字，相当于10个苹果放进9个抽屉里，一定有一个抽屉重复有两个苹果，找出那个重复的数字


## 解题思路
李永芳二分的思想，并将时间转为空间，取数组的中间值，正常来说数组中小于中间值的个数应该等于n/2，如果数组中小于中间值的个数大于一半，那么重复的数字一定小于中间值，改变右边界
注意：本题中改变的右边界不是无序数组中的边界，而是改变n个数的有序数组中的边界值


## 代码实现

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int ans=-1;
        for(int i=0,j=nums.size()-1;i<=j;){
            int mid=(i+j)/2;
            int cnt=0;
            for(int k=0;k<nums.size();k++){
                cnt+=nums[k]<=mid;
            }
            if(cnt<=mid){
                i=mid+1;
            }else{
                j=mid-1;
                ans=mid;
            }
        }
        return ans;
    }
};
```
# 力扣155最小栈


设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

实现 MinStack 类:

MinStack() 初始化堆栈对象。
void push(int val) 将元素val推入堆栈。
void pop() 删除堆栈顶部的元素。
int top() 获取堆栈顶部的元素。
int getMin() 获取堆栈中的最小元素。
 

示例 1:

输入：
`["MinStack","push","push","push","getMin","pop","top","getMin"]`
`[[],[-2],[0],[-3],[],[],[],[]]`

输出：
`[null,null,null,null,-3,null,0,-2]`

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
 

提示：

-231 <= val <= 231 - 1
pop、top 和 getMin 操作总是在 非空栈 上调用
push, pop, top, and getMin最多被调用 3 * 104 次


## 题目分析
利用栈实现 MinStack 类:

MinStack() 初始化堆栈对象。
void push(int val) 将元素val推入堆栈。
void pop() 删除堆栈顶部的元素。
int top() 获取堆栈顶部的元素。
int getMin() 获取堆栈中的最小元素。


## 解题思路
我们可以设计一个辅助栈，在每一个新的元素进入栈时记录当前栈内最小的元素将其位于栈顶，每次入栈时将其与辅助栈栈顶比较，并将最小值入栈，就可以不断记录堆栈的最小值


## 代码实现

```cpp
class MinStack {
    stack<int>x_stack;
    stack<int>min_stack;//辅助栈 用于存储任意时刻栈内元素的最小值 位于栈顶
public:
    MinStack() {
        min_stack.push(INT_MAX);
    }
    
    void push(int val) {
        x_stack.push(val);
        min_stack.push(min(min_stack.top(),val));
    }
    
    void pop() {
        x_stack.pop();
        min_stack.pop();
    }
    
    int top() {
        return x_stack.top();
    }
    
    int getMin() {
        return min_stack.top();
    }
};
