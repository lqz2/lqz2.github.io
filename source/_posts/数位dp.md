---
title: 数位dp
date: 2022-10-23 12:07:19
categories: 机试
math: true
tags:
---

<!-- TOC -->

- [数位DP](#数位dp)
    - [例题](#例题)
    - [思路](#思路)
    - [AC代码](#ac代码)

<!-- /TOC -->
## 数位DP

>一般指在限定条件下，关于每位数字出现次数的相关题目，先从入门开始吧(:cry:自己tcl)

### 例题
[P2602 [ZJOI2010] 数字计数](https://www.luogu.com.cn/problem/P2602)


**题目描述**

给定两个正整数 $a$ 和 $b$，求在 $[a,b]$ 中的所有整数中，每个数码(digit)各出现了多少次。

**输入格式**

仅包含一行两个整数 $a,b$，含义如上所述。

**输出格式**

包含一行十个整数，分别表示 $0\sim 9$ 在 $[a,b]$ 中出现了多少次。


样例输入 

```
1 99
```

样例输出 

```
9 20 20 20 20 20 20 20 20 20
```

**数据规模与约定**

- 对于 $30\%$ 的数据，保证 $a\le b\le10^6$；
- 对于 $100\%$ 的数据，保证 $1\le a\le b\le 10^{12}$。
### 思路

这道题主要就是找到各种规律

首先找到递推公式，如果有`i`位数字，那么对于`0～9`的每一个数字，出现的次数是一样的，所以不考虑前导0时有
`dp[i]=10*dp[i-1]+10^(i-1)`
dp[i]表示i位的数每个数字出现的次数

对于ABCD

看A000，把这个A000看成0000～1000～2000...A000对于不考虑首位每一个式子的数字的出现个数为 `A*dp[3]`。加上首位出现也就是小于A每一个数都出现了`10^3`次，再加上，我们就把A000处理完了。

另外，首位A还出现了BCD+1次呢，也就是从A000~ABCD，这个A还出现了BCD+1次，最后减去前导0的个数，就是`10^(i-1)`
### AC代码
```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll a,b;
ll cnta[10],cntb[10];
ll dp[13],ten[13];
void dig(ll x,ll* cnt)
{
	int len=0;
	vector<int> v(13,0);
	while(x)
	{
		v[++len]=x%10;
		x/=10;
	}
	for(int i=len;i>=1;--i)
	{
		//不算首位，每个数字出现了x*dp[i-1]次,x是当前最高位 
		for(int j=0;j<10;++j)
			cnt[j]+=v[i]*dp[i-1];
		//算首位，即小于x的数增加10^(i-1)个 
		for(int j=0;j<v[i];++j)
			cnt[j]+=ten[i-1];
		//首位还多出现了低位+1次
		ll tmp=0;
		for(int j=i-1;j>=1;--j)
		{
			tmp=tmp*10+v[j];
		}
		cnt[v[i]]+=tmp+1;
		//减去前导0
		cnt[0]-=ten[i-1]; 
	}
}
//dp[i]=10*dp[i-1]+10^(i-1)
int main()
{
	ll a,b;
	cin>>a>>b;
	ten[0]=1;
	//初始化dp 
	for(int i=1;i<=13;++i)
	{
		dp[i]=10*dp[i-1]+ten[i-1];
		ten[i]=10*ten[i-1];
	}
	dig(a-1,cnta);
	dig(b,cntb);
	for(int i=0;i<10;++i)
	{
		cout<<cntb[i]<<' ';
	}
	return 0;
} 
```