---
title: dp练习
date: 2022-09-15 09:14:03
categories: 机试
math:
tags:
---
<!-- TOC -->

- [不同二叉搜索树的个数](#不同二叉搜索树的个数)
- [最小花费爬楼梯](#最小花费爬楼梯)
- [最小花费](#最小花费)
- [分配宝藏](#分配宝藏)

<!-- /TOC -->

## 不同二叉搜索树的个数

**题目描述**

给定一个由节点值从 1 到 n 的 n 个节点。请问由多少种不同的方法用这 n 个节点构成互不相同的二叉搜索树。

仅一行输入一个正整数 n ，表示节点的数量。

输出组成不同二叉搜索树的方法数。

input:
```
3
```
output:
```
5
```
**思路**

`dp[k]`表示`k`个节点时不同二叉搜索树的个数，当有`n`个节点时，以`i`为根节点的左子树有`i-1`个节点，而右子树有`i-j`个节点，所以以`i`为根节点的二叉搜索树一共有`dp[i-1]*d[i-j]`个

代码：
```
#include <bits/stdc++.h>
using namespace std;
int dp[20];
int main() 
{
    int n;
    cin>>n;
    dp[0]=1;
    dp[1]=1;
    for(int i=2;i<=n;++i)
    {
        for(int j=1;j<=n;++j)
        {
            dp[i]+=dp[j-1]*dp[i-j];
        }
    }
    cout<<dp[n]<<endl;
    return 0;
}
```
## 最小花费爬楼梯

**题目描述**

给定一个整数数组 `cost[]` ，其中 `cost[i]`是从楼梯第`i`个台阶向上爬需要支付的费用，下标从0开始。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

input:
```
10
1 100 1 1 1 90 1 1 80 1
```
output:
```
6
```
**思路**

`dp[i]`表示第`i`个台阶之前的最小花费
因为一次能走一级或者两级台阶，所以`dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2])`

代码：
```
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+1;
int cost[N];
int dp[N];
int main()
{
    int n;
    cin>>n;
    for(int i=1;i<=n;++i)
        cin>>cost[i];
    for(int i=3;i<=n+1;++i)
    {
        dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
    }
    cout<<dp[n+1]<<endl;
    return 0;
}
```
## 最小花费

**题目描述**

在某条线路上有N个火车站，有三种距离的路程，L1，L2，L3,对应的价格为C1,C2,C3.
 每两个站之间的距离不超过L3。 当乘客要移动的两个站的距离大于L3的时候，可以选择从中间一个站下车，然后买票再上车，所以乘客整个过程中至少会买两张票。 现在给你一个 L1，L2，L3，C1，C2，C3。然后是A，B的值，其分别为乘客旅程的起始站和终点站。 然后输入N，N为该线路上的总的火车站数目，然后输入N-1个整数，分别代表从该线路上的第一个站，到第2个站，第3个站，……，第N个站的距离。 根据输入，输出乘客从A到B站的最小花费。

 **输入描述**

 第一行输入l1,l2,l3,c1,c2,c3
 第二行输入a,b
 第三行输入n
 第四行输入n-1个数，表示第一站到后面站的距离

 input:
```
1 2 3 1 2 3
1 2
2
2
```
 output:
 ```
 2
 ```

**代码**
```
#include <bits/stdc++.h>
using namespace std;
const int N=1e5;
int l1,l2,l3,c1,c2,c3;
int a,b;
int dis[N];
int dp[N];
int getprice(int a,int b)
{
    if(dis[b]-dis[a]<=l1)
        return c1;
    else if(dis[b]-dis[a]<=l2)
        return c2;
    else
        return c3;
}
int main()
{
    int n;
    while(cin>>l1>>l2>>l3>>c1>>c2>>c3)
    {
        cin>>a>>b;
        cin>>n;
        for(int i=2;i<=n;++i)
            cin>>dis[i];
        memset(dp,0x3f,sizeof(dp));
        dp[a]=0;
        for(int i=a+1;i<=b;++i)
        {
            for(int j=a;j<i;++j)
            {
                if(dis[i]-dis[j]<=l3)
                    dp[i]=min(dp[i],dp[j]+getprice(j,i));
            }
        }
        cout<<dp[b]<<endl;
        
    }
    return 0;
}
```
## 分配宝藏

**题目描述**

两个寻宝者找到一个宝藏，里面包含n件物品，每件物品的价值分别是W[0]，W[1]，…W[n-1]。
SumA代表寻宝者A所获物品价值总和，SumB代表寻宝者B所获物品价值总和，请问怎么分配才能使得两人所获物品价值总和差距最小，即两人所获物品价值总和之差的绝对值|SumA - SumB|最小。

输入说明
输入数据由两行构成：
第一行为一个正整数n，表示物品个数，其中`0<n<=200`。
第二行有n个正整数，分别代表每件物品的价值`W[i]`，其中`0<W[i]<=200`。

input:
```
4
1 2 3 4
```
output:
```
0
```

**思路**
其实是一道**01背包问题**，只需要将背包容量设置为总价值的一半即可

代码
```
#include<bits/stdc++.h>
using namespace std;
const int N=1e6+1;
int a[N];
int dp[N]; 
int main()
{
	int n;
	cin>>n;
	int sum=0;
	for(int i=1;i<=n;++i)
	{
		cin>>a[i];
		sum+=a[i];
	}
	int v=sum/2;
	for(int i=1;i<=n;++i)
	{
		for(int j=v;j>=a[i];--j)
		{
			dp[j]=max(dp[j],dp[j-a[i]]+a[i]);
		}
	}
    cout<<abs(2*dp[v]-sum)<<endl;
	return 0;
} 
```
