---
title: 每日一题-滑动窗口与二分练习
date: 2024-04-14 23:44:09
updated: 2024-07-22 16:27:42
categories: 算法
tags: [算法, 数据结构]
---
# 力扣LCR008长度最小的子数组
给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, …, numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

示例 1：

输入：`target = 7, nums = [2,3,1,2,4,3]`
输出：`2`
解释：子数组 `[4,3]` 是该条件下的长度最小的子数组。


示例 2：

输入：`target = 4, nums = [1,4,4]`
输出：`1`


示例 3：

输入：`target = 11, nums = [1,1,1,1,1,1,1,1]`
输出：`0`

## 题目分析

找出大于等于目标值的长度最小的连续的子数组

## 解题思路

利用滑动窗口的思想，先找到第一个大于等于目标值的子数组，再使窗口向右移动，当和再次大于目标值时，移动窗口并更新最小长度

## 代码实现

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n=nums.size();
        if(n==0)return {};
        int sum=0;
        int start=0,end=0,ans=INT_MAX;
        while(end<n){
            sum+=nums[end++];
            while(sum>=target){
                ans=min(ans,end-start);
                sum-=nums[start];
                start++;
            }
        }
        return ans==INT_MAX?0:ans;
    }
};

```
# 力扣LCR009乘积小于k的子数组

给定一个正整数数组 nums和整数 k ，请找出该数组内乘积小于 k 的连续的子数组的个数。

示例 1:

输入: `nums = [10,5,2,6], k = 100`
输出: `8`
解释: 8 个乘积小于 100 的子数组分别为: `[10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]`。
需要注意的是 `[10,5,2]` 并不是乘积小于100的子数组。


示例 2:

输入: `nums = [1,2,3], k = 0`
输出: `0`

## 题目分析

找出数组内乘积小于 k 的连续的子数组的个数

## 解题思路

利用滑动窗口的思想，先找到第一个长度最长的乘积小于k的数组，这样该数组里各个连续的数组都成立，然后移动窗口，继续寻找

## 代码实现

```cpp
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int sum=1,cnt=0,j=0;
        for(int i=0;i<nums.size();i++){
            sum*=nums[i];
                while(sum>=k&&j<=i){
                    sum/=nums[j++];
                }
                cnt+=i-j+1;
        }
        return cnt;
    }
};

```
# 力扣35搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

示例 1:

输入:` nums = [1,3,5,6], target = 5`
输出: `2`


示例 2:

输入: `nums = [1,3,5,6], target = 2`
输出: `1`


示例 3:

输入: `nums = [1,3,5,6], target = 7`
输出: `4`

## 题目分析

题目很好理解，在数组中寻找一个数，如果没有则返回插入位置

## 解题思路

利用二分的思想快速找到位置，因为数组严格递增，如果数组中间值大于目标值，那么右半边数组的所有数都大于目标值，可直接缩小搜索范围，同理左边，便能很快定位

## 代码实现

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left=0;
        int right=nums.size()-1;
        while(left<=right)
        {
            int middle=left+(right-left)/2;
            if(nums[middle]>target)
            {
                right=middle-1;
            }else if(nums[middle]<target)
            {
                left=middle+1;
            }else{
                return middle;
            }
        }
return right+1;
    }
};

```
# 力扣33搜索旋转排序数组

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], …, nums[n-1], nums[0], nums[1], …, nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。

示例 1：

输入：`nums = [4,5,6,7,0,1,2], target = 0`
输出：`4`


示例 2：

输入：`nums = [4,5,6,7,0,1,2], target = 3`
输出：`-1`


示例 3：

输入：`nums = [1], target = 0`
输出：`-1`

提示：

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
nums 中的每个值都 独一无二
题目数据保证 nums 在预先未知的某个下标上进行了旋转
-104 <= target <= 104

## 题目分析

给定一个升序的数组和目标值，在数组经过向右旋转多次后，搜索目标值

## 解题思路

利用二分法，因为原来的数组是升序的，即使经过旋转后，数组仍然呈现两部分的严格递增，且前一部分的初始值一定大于另一部分的末尾值，这时我们可以分为两种情况：
1.首先将旋转后的数组第一个值与中间值比较，如果中间值大，那么旋转后的分割点(也就是原数组的nums[0])在中间值以后，那么我们可以保证中间值前面都是严格递增
然后再利用二分，当目标值小于中间值时，改变右边界，注意：改变的同时一定要保证目标值也小于旋转后的nums[0]因为它有可能在后半部分
2.当中间值小于数组第一个值时，分割点在中间值前面，同理可以保证中间值后面都是严格递增的


## 代码实现

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n=nums.size();
        if(!n)return -1;
        if(n==1)return nums[0]==target?0:-1;
        int l=0,r=n-1;
        while(l<=r){
            int mid=(l+r)/2;
            if(nums[mid]==target)return mid;
            if(nums[0]<=nums[mid]){
                if(target<nums[mid]&&target>=nums[0]){
                    r=mid-1;
                }else{
                    l=mid+1;
                }
            }else{
                if(target>nums[mid]&&target<=nums[n-1]){
                    l=mid+1;
                }else{
                    r=mid-1;
                }
            }
        }
        return -1;
    }
};

```

