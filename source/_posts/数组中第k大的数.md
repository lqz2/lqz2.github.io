---
title: 数组中第k大的数
date: 2025-04-13 17:32:19
categories: 机试
math:
tags:
---
## 数组中第k大的数

<!-- TOC -->

- [数组中第k大的数](#数组中第k大的数)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)


### 题目描述
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```
### 思路
之前第一遍直接拿堆做了，实际上用快排可以达到O(n)的复杂度

思路就是快排，每次快排后，pivot左边的数都小于它，pivot右边的数都大于它，所以pivot的位置就固定了
因此，当pivot==n-k时，对应的数就是第k大的数
与快排不同的是，每次移动后，不用同时对左右两个区间递归，而是选择n-k所在的区间递归，有点像二分查找

### 代码
```
class Solution {
public:
    int adjust(vector<int>& nums,int l,int r, int target){
        int tl=l,tr=r;
        int t=nums[l];
        while(l<r){
            while(l<r&&nums[r]>=t)
                --r;
            nums[l]=nums[r];
            while(l<r&&nums[l]<=t)
                ++l;
            nums[r]=nums[l];
        }
        nums[l]=t;
        if(l==target)
            return nums[l];
        else if(l<target)
            return adjust(nums,l+1,tr,target);
        else
            return adjust(nums,tl,l-1,target);
    }
    
    int findKthLargest(vector<int>& nums, int k) {
        int n=nums.size();
        return adjust(nums,0,n-1,n-k);
    }
};
```