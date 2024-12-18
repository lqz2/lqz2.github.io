---
title: 和为k的子数组
date: 2024-12-18 10:18:10
categories:
math:
tags:
---
## 和为k的子数组

<!-- TOC -->

- [和为k的子数组](#和为k的子数组)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/subarray-sum-equals-k/description)


### 题目描述
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。
子数组是数组中元素的连续非空序列。

```
示例 1：
输入：nums = [1,1,1], k = 2
输出：2
```
### 思路
自己只想到了`O(n^2)`的解法，但是使用前缀和可以将时间复杂度降低到`O(n)`。
假设前缀和数组为sum[]，那么当`sum[i] - sum[j] = k`时，说明从j到i的子数组满足条件，实际上就是求`sum[j] = sum[i] - k`的个数。
我们可以用一个变量sum记录累加到当前位置的所有元素之和，用一个哈希表记录sum出现的次数，此时前缀和为sum-k的个数就是满足条件的个数。

### 代码
```
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n=nums.size();
        unordered_map<int,int> mp;
        mp[0]=1;
        int sum=0;
        int ans=0;
        for(int i=0;i<n;++i){
            sum+=nums[i];
            ans+=mp[sum-k];
            ++mp[sum];
        }
        return ans;
    }
};
```