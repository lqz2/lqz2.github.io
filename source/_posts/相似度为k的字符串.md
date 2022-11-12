---
title: 相似度为k的字符串
date: 2022-09-23 15:39:20
categories: 机试
math:
tags:
---
<!-- TOC -->

- [相似度为k的字符串](#相似度为k的字符串)

<!-- /TOC -->
### 相似度为k的字符串

[题目链接](https://leetcode.cn/problems/k-similar-strings/)

**题目描述**

对于某些非负整数 `k` ，如果交换 `s1` 中两个字母的位置恰好 `k` 次，能够使结果字符串等于 `s2` ，则认为字符串 `s1` 和 `s2` 的 相似度为` k` 。

给你两个字母异位词 `s1 `和 `s2` ，返回 `s1 `和` s2 `的相似度` k `的最小值。

**思路**

>主要是BFS+剪枝，当s1和s2在某个位置出现s1[i] != s2[i]时，将s1的指针后移，找到j使得s1[j] == s2[i],然后交换s1[i]和s1[j]，将字符串和位置加入队列中。同时要注意剪枝，当s1[j]==s2[j]时说明本来就能匹配，所以无需交换，当交换后的字符串在之前已经出现时，无需再次加入队列。

**代码**

```
typedef queue<pair<string,int>> qpi;
class Solution {
public:
    unordered_set<string> vis;
    int kSimilarity(string s1, string s2) {
        qpi q;
        int len=s1.size();
        q.push({s1,0});
        vis.insert(s1);
        string ts;
        int idx;
        for(int ans=0;;++ans)
        {
            int sz=q.size();
            while(sz--)
            {
                pair<string,int> t=q.front();
                q.pop();
                ts=t.first;
                idx=t.second;
                if(ts==s2)
                    return ans;
                while(idx<len&&ts[idx]==s2[idx])
                    ++idx;
                for(int i=idx+1;i<len;++i)
                {
                    if(ts[i]!=s2[i]&&ts[i]==s2[idx])
                    {
                        swap(ts[idx],ts[i]);
                        if(!vis.count(ts))
                        {
                            q.push({ts,idx+1});
                            vis.insert(ts);
                        }
                        swap(ts[idx],ts[i]);
                    }
                }

            }
            
        }
        return 0;
    }
};
```