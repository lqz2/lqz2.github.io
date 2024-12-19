---
title: 有序矩阵中第K小的元素
date: 2024-12-19 10:28:49
categories: 机试
math:
tags:
---
## 有序矩阵中第K小的元素

<!-- TOC -->

- [有序矩阵中第K小的元素](#有序矩阵中第k小的元素)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/description/)


### 题目描述
给你一个 n x n 矩阵 matrix ，其中每行和每列元素均按升序排序，找到矩阵中第 k 小的元素。
请注意，它是 排序后 的第 k 小元素，而不是第 k 个 不同 的元素。

你必须找到一个内存复杂度优于 `O(n^2)` 的解决方案。

```
示例 1：

输入：matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
输出：13
解释：矩阵中的元素为 [1,5,9,10,11,12,13,13,15]，第 8 小元素是 13
```
### 思路
这道题一般一眼看上去都会想到堆。但是堆并没有很好利用矩阵的有序性，这里采用二分查找的方法。

首先要解决的是计数问题，给定一个数 mid，如何计算矩阵中小于等于这个数的元素个数？
这里采用的是从左下角开始遍历
- 如果当前元素小于等于mid，那么这一列的元素都小于等于mid，计数加上列数，然后向右移动一列。
- 如果当前元素大于mid，计数不变，然后向上移动一行。

接下来就是二分查找了，要求找出第k小元素，可以先找出中间数mid，当小于等于mid的元素个数小于k时，说明mid比要找的数小，所以要向右移动，否则向左移动。
计数的方法很巧妙，值得学习。
### 代码
```
class Solution {
public:
    int count(vector<vector<int>>& matrix,int mid,int n){
        int i=n-1,j=0;//从左下角开始
        int cnt=0;
        while(i>=0&&j<n){
            if(matrix[i][j]<=mid){
                cnt+=i+1;
                ++j;
            }else
                --i;
        }
        return cnt;
    }

    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n=matrix.size();
        int l=matrix[0][0],r=matrix[n-1][n-1];
        while(l<r){
            int mid=l+(r-l)/2;
            if(count(matrix,mid,n)>=k){
                r=mid;
            }else{
                l=mid+1;
            }
        }
        return l;
    }
};
```