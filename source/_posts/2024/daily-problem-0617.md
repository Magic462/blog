---
title: 动态规划
date: 2024-05-20 17:20:29
updated: 2024-05-21 13:14:09
categories: 算法
tags: [算法, 数据结构]
---
# 力扣740删除并获得整数

给你一个整数数组 nums ，你可以对它进行一些操作。

每次操作中，选择任意一个 nums[i] ，删除它并获得 nums[i] 的点数。之后，你必须删除 所有 等于 nums[i] - 1 和 nums[i] + 1 的元素。

开始你拥有 0 个点数。返回你能通过这些操作获得的最大点数。

 

示例 1：

输入：nums = `[3,4,2]`

输出：6

解释：
删除 4 获得 4 个点数，因此 3 也被删除。
之后，删除 2 获得 2 个点数。总共获得 6 个点数。


示例 2：

输入：nums = `[2,2,3,3,3,4]`

输出：9

解释：
删除 3 获得 3 个点数，接着要删除两个 2 和 4 。
之后，再次删除 3 获得 3 个点数，再次删除 3 获得 3 个点数。
总共获得 9 个点数。
 

提示：

1 <= nums.length <= 2 * 104
1 <= nums[i] <= 104

## 题目分析

选择一个任意点数nums[i]删除并获得他的点数，同时删除等于nums[i]+1和nums[i]-1的所有点数，此次删除并不能获得点数，即损失了他们的点数，求能获得的最大点数

## 解题思路

利用动态规划的思想
当我们选择了一个点数之后，要把这个点数都选完才能最大减少损失，并且这个点数的左右相邻点数都将损失，即我们不能选择连续的点数
听到这里大家有没有一丝熟悉的感觉，没错，与打家劫舍的问题类似
1. 首先我们求出数组中的最大点数，另定义一个数组sum用来存放每个点数与其出现次数的乘积
2. 令定义一个函数与打家劫舍源码一样
3. 将sum代入函数中，返回动态规划的值

## 代码实现

```cpp
class Solution {
private:
    int rob(vector<int>&nums){
        int n=nums.size();
        vector<int>dp(n+1,0);
        dp[0]=0;
        dp[1]=nums[0];
        for(int i=2;i<=n;i++)
        {
            dp[i]=max(dp[i-1],dp[i-2]+nums[i-1]);
        }
        return dp[n];
    }
public:
    int deleteAndEarn(vector<int>& nums) {
        int maxval=0;
        for(int val:nums)
        {
            maxval=max(maxval,val);
        }
        vector<int>sum(maxval+1);
        for(int val:nums)
        {
            sum[val]+=val;
        }
        return rob(sum);
    }
};
```

# 力扣1173第N个泰波那契数

泰波那契序列 Tn 定义如下： 

T0 = 0, T1 = 1, T2 = 1, 且在 n >= 0 的条件下 Tn+3 = Tn + Tn+1 + Tn+2

给你整数 n，请返回第 n 个泰波那契数 Tn 的值。

 

示例 1：

输入：n = 4

输出：4

解释：
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4


示例 2：

输入：n = 25

输出：1389537
 
提示：

0 <= n <= 37
答案保证是一个 32 位整数，即 answer <= 2^31 - 1。

## 题目分析

根据公式求得第n个数

## 解题思路

利用动态规划的思想
注意数组越界的问题，初始化定义dp数组的大小要为n+3
**优化**可以用滚动数组的思想

## 代码实现

```cpp
class Solution {
public:
    int tribonacci(int n) {
        vector<int>dp(n+3,0);
        dp[0]=0;
        dp[1]=1;
        dp[2]=1;
        for(int i=3;i<=n;i++)
        {
            dp[i]=dp[i-1]+dp[i-2]+dp[i-3];
        }
        return dp[n];
    }
};
```
## 代码优化

```cpp
class Solution {
    public int tribonacci(int n) {
        if (n == 0) {
            return 0;
        }
        if (n <= 2) {
            return 1;
        }
        int p = 0, q = 0, r = 1, s = 1;
        for (int i = 3; i <= n; ++i) {
            p = q;
            q = r;
            r = s;
            s = p + q + r;
        }
        return s;
    }
}
```