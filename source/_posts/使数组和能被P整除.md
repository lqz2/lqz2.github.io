---
title: 使数组和能被P整除
date: 2023-03-10 08:11:13
categories: 机试
math:
tags:
---
<!-- TOC -->

- [使数组和能被 P 整除](#使数组和能被-p-整除)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 使数组和能被 P 整除
[原题](https://leetcode.cn/problems/make-sum-divisible-by-p/description/)
### 题目描述
给你一个正整数数组 nums，请你移除 最短 子数组（可以为 空），使得剩余元素的 和 能被 p 整除。 不允许 将整个数组都移除。

请你返回你需要移除的最短子数组的长度，如果无法满足题目要求，返回 -1 。

子数组 定义为原数组中连续的一组元素。

e.g.
```
输入：nums = [3,1,4,2], p = 6
输出：1
解释：nums 中元素和为 10，不能被 p 整除。我们可以移除子数组 [4] ，剩余元素的和为 6 
```
### 思路
设整个数组的和对p取模的值为`x`，前缀和(经过取模处理)数组为`sum`，若要满足去掉`nums[i]~nums[j]`之间的数后，剩下的数之和能被p整除，则满足`(sum[j]-x+p)%p==sum[i]`,为了计算最短长度，还需要用哈系表记录当前位置`sum[j]`的下标j，在代码实现时，`sum`数组可以通过维护一个整型值代替。

### 代码
```
class Solution {
public:
    int minSubarray(vector<int>& nums, int p) {
        int x=0;
        for(int a:nums)
            x=(x+a)%p;
        if(x==0)
            return 0;
        unordered_map<int,int> idx;
        int y=0,res=nums.size(),n=nums.size();
        for(int i=0;i<n;++i)
        {
            idx[y]=i;
            y=(y+nums[i])%p;
            if(idx.count((y-x+p)%p)>0)
                res=min(res,i-idx[(y-x+p)%p]+1);
        }
        return res==n?-1:res;
    }
};
```