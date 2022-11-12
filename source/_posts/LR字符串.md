---
title: LR字符串
date: 2022-10-02 15:12:39
categories: 机试
math:
tags:
---
<!-- TOC -->

- [在LR字符串中交换相邻字符](#在lr字符串中交换相邻字符)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 在LR字符串中交换相邻字符
[原题链接](https://leetcode.cn/problems/swap-adjacent-in-lr-string/)

### 题目描述

在一个由 'L' , 'R' 和 'X' 三个字符组成的字符串（例如"RXXLRXRXL"）中进行移动操作。一次移动操作指用一个"LX"替换一个"XL"，或者用一个"XR"替换一个"RX"。现给定起始字符串start和结束字符串end，请编写代码，当且仅当存在一系列移动操作使得start可以转换成end时， 返回True。

input:
```
start = "RXXLRXRXL", end = "XRLXXRRLX"
```

output:
```
true
```

### 思路

用 i 和 j 分别表示 start 和 end 中的下标，跳过所有的`X`，当`start[i]!=start[j]`,直接返回false,当`start[i]==start[j]`，如果字符是`L`,应满足`j<=i`,如果字符是`R`,应满足`i<=j`，否则返回`false`

### 代码

```
class Solution {
public:
    bool canTransform(string start, string end) {
        int n=start.size();
        int i=0,j=0;
        while(i<n&&j<n)
        {
            while(i<n&&start[i]=='X')
                ++i;
            while(j<n&&end[j]=='X')
                ++j;
            if(i<n&&j<n)
            {
                if(start[i]!=end[j])
                    return false;
                else
                {
                    if(start[i]=='L'&&i<j)
                        return false;
                    if(start[i]=='R'&&i>j)
                        return false;
                }
                ++i;
                ++j;
            }
        }
        while(i<n)
        {
            if(start[i]!='X')
                return false;
            ++i;
        }
        while(j<n)
        {
            if(end[j]!='X')
                return false;
            ++j;
        }
        return true;
    }
};
```