---
title: 第k个数
date: 2022-09-29 10:13:29
categories: 机试
math:
tags:
---
<!-- TOC -->

- [第k个数](#第k个数)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 第k个数

>此题不是特别难，但是第一眼看过去没有什么太好的想法，看了眼题解觉得思路很好，就记录下来 
~~顺便水一个博客:smile:~~

### 题目描述

有些数的素因子只有 3，5，7，请设计一个算法找出第 k 个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。例如，前几个数按顺序应该是 1，3，5，7，9，15，21。

输入k，输出符合条件的第k个数

input:
```
5
```
output:
```
9
```
### 思路

建立一个数组`dp[]`，`dp[i]`表示第`i`个数什么，然后三个变量a,b,c表示3、5、7的指针

状态方程：
```
int numa=dp[a]*3,numb=dp[b]*5,numc=dp[c]*7;
dp[i]=min(min(numa,numb),numc);
```
然后如果`dp[i]`与任何一个num相等，对应的指针加1

### 代码
```
#include<bits/stdc++.h>
using namespace std;
const int N=1e3;
int dp[N];
int main()
{
	int n;
	cin>>n;
	int a=1,b=1,c=1;
	dp[1]=1;
	for(int i=2;i<=n;++i)
	{
		int numa=dp[a]*3,numb=dp[b]*5,numc=dp[c]*7;
		dp[i]=min(min(numa,numb),numc);
		if(dp[i]==numa)
			++a;
		if(dp[i]==numb)
			++b;
		if(dp[i]==numc)
			++c;	
	}
	cout<<dp[n]<<endl;
	return 0;
}
```


