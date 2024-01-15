---
title: 价值和小于等于k的最大数字
date: 2024-01-15 10:26:13
categories:
math: true
tags:
---
<!-- TOC -->

- [价值和小于等于k的最大数字](#价值和小于等于k的最大数字)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 价值和小于等于k的最大数字
[原题](https://leetcode.cn/problems/maximum-number-that-sum-of-the-prices-is-less-than-or-equal-to-k/description/)
### 题目描述
给你一个整数 k 和一个整数 x 。

令 s 为整数 num 的下标从 1 开始的二进制表示。我们说一个整数 num 的 价值 是满足 i % x == 0 且 s[i] 是 设置位 的 i 的数目。

请你返回 最大 整数 num ，满足从 1 到 num 的所有整数的 价值 和小于等于 k 。

注意：

一个整数二进制表示下 设置位 是值为 1 的数位。
一个整数的二进制表示下标从右到左编号，比方说如果 s == 11100 ，那么 s[4] == 1 且 s[2] == 0 。

```
输入：k = 9, x = 1
输出：6
解释：数字 1 ，2 ，3 ，4 ，5 和 6 二进制表示分别为 "1" ，"10" ，"11" ，"100" ，"101" 和 "110" 。
由于 x 等于 1 ，每个数字的价值分别为所有设置位的数目。
这些数字的所有设置位数目总数是 9 ，所以前 6 个数字的价值和为 9 。
所以答案为 6 。
```
### 思路
周赛的第3题，做的时候时间复杂度优化不下来，一直超时。整体思路是数学加二分。

首先，求1～n的所有数每个位置上1的数量。这里可以用数位DP，也可以直接用数学公式推导：
对于1～n的所有数，第i(从右到左递增，从0开始)个位置的1的数量为
$$
\frac{n+1}{2^{i+1}} \times 2^{i} + max(0,(n+1) \% 2^{i+1}-2^i)
$$
可以通过位运算完成上面的计算，比如`n mod 2^i`即`n & ((1 << i) - 1)`

所以上面的式子可表示为：
```
(((n + 1LL) >> (i + 1LL)) << i) + max(0LL, ((n + 1LL) & ((1LL << (i + 1)) - 1LL)) - (1LL << i))
```
这样我们可以快速计算1～n的价值和，得到价值和后需要用二分查找得到价值和小于等于K的最大的n，因为价值和是递增的，所以也就是找到价值和大于K的n再减去1，也就是价值和大于等于K+1的n减去1，下面进行二分查找即可。
### 代码
```
class Solution
{
public:
    // 判断1~n的价值和是否超过了k
    bool check(long long n, int x, long long k)
    {
        long long res = 0;
        int len = 64 - __builtin_clzll(n);
        for (int i = 0; i < len; ++i)
        {
            if ((i + 1) % x == 0)
            {
                res += (((n + 1LL) >> (i + 1LL)) << i) + max(0LL, ((n + 1LL) & ((1LL << (i + 1)) - 1LL)) - (1LL << i));
            }
        }
        return res < k + 1;
    }
    long long findMaximumNumber(long long k, int x)
    {
        long long l = 1, r = (k + 1) << x;
        while (l <= r)
        {
            long long mid = l + (r - l) / 2;
            if (check(mid, x, k))
                l = mid + 1;
            else
                r = mid - 1;
        }
        return l - 1;
    }
};
```