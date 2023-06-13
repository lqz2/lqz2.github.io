---
title: 最近公共祖先LCA
date: 2023-06-13 13:37:41
categories: 机试
math:
tags:
---
<!-- TOC -->

- [树节点的第K个祖先](#树节点的第k个祖先)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->
## 树节点的第K个祖先
[原题](https://leetcode.cn/problems/kth-ancestor-of-a-tree-node/description/)
### 题目描述
给你一棵树，树上有 n 个节点，按从 0 到 n-1 编号。树以父节点数组的形式给出，其中 parent[i] 是节点 i 的父节点。树的根节点是编号为 0 的节点。

树节点的第 k 个祖先节点是从该节点到根节点路径上的第 k 个节点。

实现 TreeAncestor 类：

TreeAncestor（int n， int[] parent） 对树和父数组中的节点数初始化对象。
getKthAncestor(int node, int k) 返回节点 node 的第 k 个祖先节点。如果不存在这样的祖先节点，返回 -1 。

e.g.

```
输入：
["TreeAncestor","getKthAncestor","getKthAncestor","getKthAncestor"]
[[7,[-1,0,0,1,1,2,2]],[3,1],[5,2],[6,3]]

输出：
[null,1,0,-1]

解释：
TreeAncestor treeAncestor = new TreeAncestor(7, [-1, 0, 0, 1, 1, 2, 2]);
treeAncestor.getKthAncestor(3, 1);  // 返回 1 ，它是 3 的父节点
treeAncestor.getKthAncestor(5, 2);  // 返回 0 ，它是 5 的祖父节点
treeAncestor.getKthAncestor(6, 3);  // 返回 -1 因为不存在满足要求的祖先节点
```
### 思路
主要用到树上倍增算法，需要用二维数组`pa[x][i]`表示节点`x`的第`2^i`祖先节点，则满足`pa[x][i+1]=pa[pa[x][i]][i]`，然后获取第k个祖先节点时，将k转换成二进制然后按位计算即可。
### 代码
```
class TreeAncestor {
    vector<vector<int>> pa;
public:
    TreeAncestor(int n, vector<int>& parent) {
        int m=32-__builtin_clz(n);
        pa.resize(n,vector<int>(m,-1));
        for(int i=0;i<n;++i)
            pa[i][0]=parent[i];
        for(int i=0;i<m-1;++i)
        {
            for(int x=0;x<n;++x)
            {
                if(int p=pa[x][i];p!=-1)
                    pa[x][i+1]=pa[p][i];
            }
        }
    }  
    int getKthAncestor(int node, int k) {
        int l=32-__builtin_clz(k);
        for(int i=0;i<l;++i)
        {
            if((k>>i)&1)
            {
                node=pa[node][i];
                if(node<0)
                    break;
            }
        }
        return node;    
    }
};
 ```
## 最近公共祖先LCA
### 思路
关于两个点x和y的lca，可以先预处理出每个节点的深度数组`depth[i]`，由于x和y的深度可能不同，所以需要现将深度较深的一个如`y`转换成它和x同一深度的祖先`y'`，然后x和y同时往上跳，在x到根节点的这条路径上猜一个点z当作lca，且x与z相距`2^i`步。把x和y同时向上跳`2^i`步，如果`x≠y`，就说明lca在z的上面，否则lca要么是z，要么在z的下面。

下面是模板：
```
class TreeAncestor {
    vector<int> depth;
    vector<vector<int>> pa;
public:
    TreeAncestor(vector<pair<int, int>> &edges) {
        int n = edges.size() + 1;
        int m = 32 - __builtin_clz(n); // n 的二进制长度
        vector<vector<int>> g(n);
        for (auto [x, y]: edges) { // 节点编号从 0 开始
            g[x].push_back(y);
            g[y].push_back(x);
        }

        depth.resize(n);
        pa.resize(n, vector<int>(m, -1));
        function<void(int, int)> dfs = [&](int x, int fa) {
            pa[x][0] = fa;
            for (int y: g[x]) {
                if (y != fa) {
                    depth[y] = depth[x] + 1;
                    dfs(y, x);
                }
            }
        };
        dfs(0, -1);

        for (int i = 0; i < m - 1; i++)
            for (int x = 0; x < n; x++)
                if (int p = pa[x][i]; p != -1)
                    pa[x][i + 1] = pa[p][i];
    }

    int get_kth_ancestor(int node, int k) {
        for (; k; k &= k - 1)
            node = pa[node][__builtin_ctz(k)];
        return node;
    }

    // 返回 x 和 y 的最近公共祖先（节点编号从 0 开始）
    int get_lca(int x, int y) {
        if (depth[x] > depth[y])
            swap(x, y);
        // 使 y 和 x 在同一深度
        y = get_kth_ancestor(y, depth[y] - depth[x]);
        if (y == x)
            return x;
        for (int i = pa[x].size() - 1; i >= 0; i--) {
            int px = pa[x][i], py = pa[y][i];
            if (px != py) {
                x = px;
                y = py;
            }
        }
        return pa[x][0];
    }
};
```
