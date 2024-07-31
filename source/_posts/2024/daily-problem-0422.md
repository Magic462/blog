---
title: 移动应用开发实验室大一上考核题分析
date: 2024-04-21 17:20:09
updated: 2024-04-22 13:44:36
categories: 算法
tags: [算法, 数据结构]
---
# 力扣125验证回文串

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 回文串 。

字母和数字都属于字母数字字符。

给你一个字符串 s，如果它是 回文串 ，返回 true ；否则，返回 false 。

 

示例 1：

输入:s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。


示例 2：

输入：s = "race a car"
输出：false
解释："raceacar" 不是回文串。


示例 3：

输入：s = " "
输出：true
解释：在移除非字母数字字符之后，s 是一个空字符串 "" 。
由于空字符串正着反着读都一样，所以是回文串。


## 题目分析

将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 回文串 。

## 解题思路

利用快慢指针遍历字符串，用isalnum函数判断字符串里是否是数字或字母，当遇到大写字母时转化为小写，一旦不相等返回false，直至快慢指针相遇，跳出循环

## 代码实现

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int j=s.size()-1;
        int i=0;
        while(i<j)
        {
           while (!isalnum(s[i])&&i<j)
            {
               i++;
            }
                if(s[i]>='A'&&s[i]<='Z')
                {
                    s[i]=s[i]+32;
                }
                while(!isalnum(s[j])&&i<j)
                {
                    j--;
                }
                    if(s[j]>='A'&&s[j]<='Z')
                    {
                        s[j]=s[j]+32;
                    }
                    if(s[i]==s[j])
                    {
                        i++;
                        j--;
                    }
                    else{return false;}
        }
        return true;
    }
};
```

# 力扣73矩阵置零

给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

 

示例 1：


输入：matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
输出：`[[1,0,1],[0,0,0],[1,0,1]]`


示例 2：


输入：matrix = `[[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
输出：`[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`
 

提示：

m == matrix.length
n == matrix[0].length
1 <= m, n <= 200
-231 <= matrix[i][j] <= 231 - 1

## 题目分析

给定一个矩阵，当矩阵出现0时，0所在的行与列都置0

## 解题思路

在双重循环中利用两个数组标记矩阵中0所在的行与列
再利用双重循环，如果有标记，那么那一行与列都置零

## 代码实现

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int>row(m),col(n);
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0){
                row[i]=col[j]=true;
                }
            }
        }
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(row[i]||col[j]){
                    matrix[i][j]=0;
                }
            }
        }
    }
};

```

# 力扣21合并两个有序链表

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 

示例 1：


输入：l1 = `[1,2,4]`, l2 = `[1,3,4]`
输出：`[1,1,2,3,4,4]`


示例 2：

输入：l1 = `[]`, l2 = `[]`
输出：[]


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

# 力扣147对链表进行插入排序

给定单个链表的头 head ，使用 插入排序 对链表进行排序，并返回 排序后链表的头 。

插入排序 算法的步骤:

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。
下面是插入排序算法的一个图形示例。部分排序的列表(黑色)最初只包含列表中的第一个元素。每次迭代时，从输入数据中删除一个元素(红色)，并就地插入已排序的列表中。

对链表进行插入排序。

示例 1：

输入: head = [4,2,1,3]
输出: [1,2,3,4]


示例 2：


输入: head = [-1,5,3,4,0]
输出: [-1,0,3,4,5]
 

提示：

列表中的节点数在 [1, 5000]范围内
-5000 <= Node.val <= 5000

## 题目分析

对链表进行插入排序
插入排序：从数组（链表）的第一个数开始，每次遍历数组寻找合适位置，直至数组的最后一个数排序完

## 解题思路

因为有可能在头结点之前插入结点，所以我们要设计一个虚拟头结点便于操作
首先设计三个指针
p记录要改变的结点位置
q记录改变结点的下一个位置，防止丢失结点
pre记录虚拟头结点，实现每一次遍历链表
然后
1.当p的值小于等于q的值就不需要移动结点，同时将p,q指向的结点位置向后移一位
2.当p的值大于q时，pre指针遍历链表，寻找第一个大于p值的结点，并将pre放在此节点的前一个位置，便于插入

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
    ListNode* insertionSortList(ListNode* head) {
        if (head == nullptr) {
            return head;
        }
        ListNode*dummy=new ListNode(0);
        dummy->next=head;
        ListNode*cur=dummy;
        ListNode*p=head;
        ListNode*q=head->next;
        while(q!=NULL){
            if(p->val<=q->val){
                p=p->next;
            }else {
                ListNode*pre=dummy;
                while(pre->next->val<=q->val){
                    pre=pre->next;
                }
                p->next=q->next;
                q->next=pre->next;
                pre->next=q;
            }
            q=p->next;
        }
        return dummy->next;
    }
};
```

# 力扣309买卖股票的最佳时期含冷冻时期

给定一个整数数组prices，其中第  prices[i] 表示第 i 天的股票价格 。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

示例 1:

输入: prices = `[1,2,3,0,2]`
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]


示例 2:

输入: prices = `[1]`
输出: 0
 

提示：

1 <= prices.length <= 5000
0 <= prices[i] <= 1000


## 题目分析

求得卖出股票的最大利益，由于冷冻期的存在，不能连续两天对股票进行操作，并且手里最多只能持有一只股票

## 解题思路

利用动态规划和贪心的思想
f[i][0]: 手上持有股票的最大收益
f[i][1]: 手上不持有股票，并且处于冷冻期中的累计最大收益
f[i][2]: 手上不持有股票，并且不在冷冻期中的累计最大收益
对f[i][0]分析：1.第i天无操作，最大收益为第i-1天 2.第i天买入股票那么第i-1天不持有股票且不能进入冷冻期
f[i][0]=max(f[i-1][0],f[i-1][2]-prices[i])
对f[i][1]分析：处于冷冻期所以第i天卖出了股票
f[i][1]=f[i-1][0]+prices[i]
对f[i][2]分析：第i天不在冷冻期所以第i-1天没操作，又不持有股票，所以第i-1天也没有股票，那么取第i-1天在不在冷冻期的最大值
f[i][2]=max(f[i−1][1],f[i−1][2])

## 代码实现

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) {
            return 0;
        }

        int n = prices.size();
        // f[i][0]: 手上持有股票的最大收益
        // f[i][1]: 手上不持有股票，并且处于冷冻期中的累计最大收益
        // f[i][2]: 手上不持有股票，并且不在冷冻期中的累计最大收益
        vector<vector<int>> f(n, vector<int>(3));
        f[0][0] = -prices[0];
        for (int i = 1; i < n; ++i) {
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - prices[i]);
            f[i][1] = f[i - 1][0] + prices[i];
            f[i][2] = max(f[i - 1][1], f[i - 1][2]);
        }
        return max(f[n - 1][1], f[n - 1][2]);
    }
};
```

# 力扣187重复的DNA序列

DNA序列 由一系列核苷酸组成，缩写为 'A', 'C', 'G' 和 'T'.。

例如，"ACGAATTCCG" 是一个 DNA序列 。
在研究 DNA 时，识别 DNA 中的重复序列非常有用。

给定一个表示 DNA序列 的字符串 s ，返回所有在 DNA 分子中出现不止一次的 长度为 10 的序列(子字符串)。你可以按 任意顺序 返回答案。

 

示例 1：

输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：`["AAAAACCCCC","CCCCCAAAAA"]`


示例 2：

输入：s = "AAAAAAAAAAAAA"
输出：`["AAAAAAAAAA"]`
 

提示：

0 <= s.length <= 105
s[i]=='A'、'C'、'G' or 'T'

## 题目分析

给你一串字符串，返回出现不止一次的长度为10的子串

## 解题思路

利用滑动窗口和哈希表，每次移动一个长度截取长度为10的子串，再用哈希表判断子串中的各个字母是否与之前重复，只当次数等于2时输出子串，防止输出重复子串

## 代码实现

```cpp
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> p,tmp;
        int n=s.length();
        unordered_map<string,int>cnt;
        for(int j=0;j<=n-10;j++){
            string sub=s.substr(j,10);
            if(++cnt[sub]==2){
                tmp.push_back(sub);
            }
        }
        return tmp;
    }
};
```

# 力扣2517礼盒最大甜蜜度

给你一个正整数数组 price ，其中 price[i] 表示第 i 类糖果的价格，另给你一个正整数 k 。

商店组合 k 类 不同 糖果打包成礼盒出售。礼盒的 甜蜜度 是礼盒中任意两种糖果 价格 绝对差的最小值。

返回礼盒的 最大 甜蜜度。

 

示例 1：

输入：price = [13,5,1,8,21,2], k = 3
输出：8
解释：选出价格分别为 [13,5,21] 的三类糖果。
礼盒的甜蜜度为 min(|13 - 5|, |13 - 21|, |5 - 21|) = min(8, 8, 16) = 8 。
可以证明能够取得的最大甜蜜度就是 8 。


示例 2：

输入：price = [1,3,1], k = 2
输出：2
解释：选出价格分别为 [1,3] 的两类糖果。 
礼盒的甜蜜度为 min(|1 - 3|) = min(2) = 2 。
可以证明能够取得的最大甜蜜度就是 2 。


示例 3：

输入：price = [7,7,7,7], k = 2
输出：0
解释：从现有的糖果中任选两类糖果，甜蜜度都会是 0 。
 

提示：

2 <= k <= price.length <= 105
1 <= price[i] <= 109

## 题目分析

选出k种糖果，使得这k种里任意两种糖果价格绝对差的最小值最大

## 解题思路

利用贪心和二分查找的思想
1，首先假设一个甜蜜度 mid，然后尝试在排好序的 price中找出 k 种糖果，并且任意两种相邻的价格差绝对值都大于 mid。如果可以找到这样的 k 种糖果，则说明可能存在更大的甜蜜度，返回true,修改左边界；如果找不到这样的 k 种糖果，则说明最大的甜蜜度只可能更小，返回false,修改右边界。
2.然后在假设一个甜蜜度 mid后，在排好序的 price中找 k 种糖果时，需要用到贪心的算法。即从小到大遍历 price 的元素，如果当前糖果的价格比上一个选中的糖果的价格的差大于 mid，则选中当前糖果，否则继续考察下一个糖果。

## 代码实现

```cpp
class Solution {
public:
    int maximumTastiness(vector<int>& price, int k) {
        int n=price.size();
        sort(price.begin(),price.end());
        int left = 0, right = price[n - 1] - price[0];
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(price, k, mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    bool check(const vector<int> &price, int k, int tastiness) {
        int prev = INT_MIN >> 1;
        int cnt = 0;
        for (int p : price) {
            if (p - prev >= tastiness) {
                cnt++;
                prev = p;
            }
        }
        return cnt >= k;
    }
};
```