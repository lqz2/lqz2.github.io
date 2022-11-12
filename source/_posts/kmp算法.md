---
title: kmp算法
date: 2022-08-21 22:36:40
categories: 机试
tags:
---

>***kmp*** 算法的核心思想：当匹配失败时，主串指针不回退，而是根据 ***next*** 数组，从适当的位置重新匹配，相比于暴力匹配，跳过了不可能匹配成功的部分，大大提升了效率

> ***next*** 数组每个位置的值表示前缀集合和后缀集合交集中,最长元素的长度

            p:    a b a b a b c a

        next[i]:  0 0 1 2 3 4 0 1

例题:

给你两个字符串 s 和 p ，请你在 s 字符串中找出 p 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回 -1 。

input:
```
abacdf
acd
```

output:
```
2
```

题解
```
#include<bits/stdc++.h>
using namespace std;
const int N=1e5;
int nxt[N];
void get_next(string p)
{
	int len=p.size();
	int i=1,j=0;
	while(i<len)
	{
		if(p[i]==p[j])
		{
			nxt[i]=j+1;
			++i;
			++j;
		}
		else if(j==0)
		{
			nxt[i]=0;
			++i;
		}
		else
			j=nxt[j-1];
	}
} 
int main()
{
	string s,p;
	cin>>s>>p;
	int m=s.size(),n=p.size();
	int ans=-1;
	
	for(int i=0,j=0;i<m;++i)
	{
		while(j>0&&s[i]!=p[j])
			j=nxt[j-1];
		if(s[i]==p[j])
			++j;
		if(j==n)
			ans=i-j+1;
	}
	cout<<ans<<endl;
	return 0;
}
```

