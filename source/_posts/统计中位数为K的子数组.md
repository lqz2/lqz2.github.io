---
title: 统计中位数为K的子数组
date: 2023-03-16 08:45:21
categories: 机试
math:
tags:
---
<!-- TOC -->

- [统计中位数为 K 的子数组](#统计中位数为-k-的子数组)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 统计中位数为 K 的子数组
[原题](https://leetcode.cn/problems/count-subarrays-with-median-k/description/)

### 题目描述
给你一个长度为 n 的数组 nums ，该数组由从 1 到 n 的 不同 整数组成。另给你一个正整数 k 。

统计并返回 nums 中的 中位数 等于 k 的非空子数组的数目。

注意：

数组的中位数是按 递增 顺序排列后位于 中间 的那个元素，如果数组长度为偶数，则中位数是位于中间靠 左 的那个元素。
例如，[2,3,1,4] 的中位数是 2 ，[8,4,3,5,1] 的中位数是 4 。
子数组是数组中的一个连续部分。

e.g.
```
输入：nums = [3,2,1,4,5], k = 4
输出：3
解释：中位数等于 4 的子数组有：[4]、[4,5] 和 [1,4,5] 。
```
### 思路
首先找出`k`在数组中的位置`idx`，对于每个数，大于`k`记为1,否则为-1，统计每个位置前缀和。如果下标`l`处与`r`处的前缀和之差为0或1,那么说明`[l+1,r]`范围内的子数组元素和为0或1,那么`k`就是该子数组的中位数。在统计相同前缀和个数时，用哈系表记录前缀和出现的次数，然后对符合要求的位置累加即可。

### 代码
```
class Solution {
public:
    int judge(int a,int b)
    {
        if(a>b)
            return 1;
        else if(a<b)
            return -1;
        return 0;
    }
    int countSubarrays(vector<int>& nums, int k) {
        int n=nums.size();
        int idx=0,sum=0;
        for(int i=0;i<n;++i)
        {
            if(nums[i]==k)
            {
                idx=i;
                break;
            }
        }
        int ans=0;
        unordered_map<int,int> mp;
        mp[0]=1;
        for(int i=0;i<n;++i)
        {
            sum+=judge(nums[i],k);
            if(i<idx)
                ++mp[sum];
            else
                ans+=mp[sum]+mp[sum-1];
        }
        return ans;
    }
};
```