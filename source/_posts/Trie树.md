---
title: Trie树
date: 2025-01-04 15:20:08
categories: 机试
math:
tags:
---
## Trie树

<!-- TOC -->

- [Trie树](#trie树)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/implement-trie-prefix-tree/description)


### 题目描述
Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。

请你实现 Trie 类：

Trie() 初始化前缀树对象。
void insert(String word) 向前缀树中插入字符串 word 。
boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。

```
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]
```
### 思路
这道题需要实现一个Trie树以及一些基本操作。可以维护两个成员变量：
- `vector<Trie*> next`：用于存储子节点的数组
- `bool isEnd`：用于标记是否是一个单词的结尾

由于是26个小写字母，所以`next`数组大小初始化为26, isEnd初始化为false。

插入时，遍历单词的每个字母，如果当前字母在next数组中不存在，则新建一个Trie节点`node->next[c-'a']==new Trie()`，然后移动到下一个节点`node=node->next[c-'a']`。
遍历完字符串后将`node->isEnd=true`。

查找时，遍历单词的每个字母，如果当前字母在next数组中不存在，则返回false，存在则移动到下一个节点。遍历完字符串后，返回`node->isEnd`。
### 代码
```
class Trie {
private:
    bool isEnd;
    vector<Trie*> next;
public:
    Trie(): next(26), isEnd(false) {}
    void insert(string word) {
        Trie *node=this;
        for(char c:word){
            if(node->next[c-'a']==nullptr){
                node->next[c-'a']=new Trie();
            }
            node=node->next[c-'a'];
        }
        node->isEnd=true;
    }
    
    bool search(string word) {
        Trie* node=this;
        for(char c:word){
            node=node->next[c-'a'];
            if(node==nullptr)
                return false;
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        Trie* node=this;
        for(char c:prefix){
            node=node->next[c-'a'];
            if(node==nullptr)
                return false;
        }
        return true;
    }
};

```