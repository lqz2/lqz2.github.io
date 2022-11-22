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
- [第 N 个神奇数字](#第-n-个神奇数字)
    - [题目描述](#题目描述-1)
    - [思路](#思路-1)
    - [代码](#代码-1)

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
## 第 N 个神奇数字

[原题](https://leetcode.cn/problems/nth-magical-number/description/)

### 题目描述

一个正整数如果能被 a 或 b 整除，那么它是神奇的。

给定三个整数 n , a , b ，返回第 n 个神奇的数字。因为答案可能很大，所以返回答案 对 109 + 7 取模 后的值。

示例
```
输入：n = 4, a = 2, b = 3
输出：6
```

### 思路

用到了容斥原理，对于一个数x，他包含`x/a`个数能被a整除，包含`x/b`个数能被b整除,若a和b最小公倍数为c，那么x包含`x/c`个数能被a和b同时整除，因此，能被a或b整除的数的个数为：`x/a+x/b-x/c`

根据以上规律，使用二分查找即可得出答案。

### 代码
```
const int mod=1e9+7;
class Solution {
public:
    const int mod=1e9+7;
    int gcd(int a,int b)
    {
        if(b)
            return gcd(b,a%b);
        return a;
    }
    int nthMagicalNumber(int n, int a, int b) {
        long long l=min(a,b);
        long long r=(long long)n*l;
        long long c=a*b/gcd(a,b);
        long long mid,t;
        while(l<=r)
        {
            mid=l+r>>1;
            t=mid/a+mid/b-mid/c;
            if(t<n)
                l=mid+1;
            else
                r=mid-1;
        }
        return (r+1)%mod;
    }
};
```

