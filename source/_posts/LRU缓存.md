---
title: LRU缓存
date: 2024-09-16 10:11:11
categories: 机试
math:
tags:
---
## LRU缓存
<!-- TOC -->

- [LRU缓存](#lru缓存)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/lru-cache/description)

### 题目描述
请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现 LRUCache 类：
LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。

e.g.
```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]
```
### 思路
一开始看到以O(1)时间复杂度运行，想到用一个数组和一个队列解决，但是实际上想得太简单了，忽略了一些复杂情况。官方题解用哈系表和双向队列解决，写的很复杂，我主要参考了评论区的大佬的思路。

首先定义一个结构体Node，包括Key, value, prev指针, next指针，用来代表一个双向链表中的节点。

对该结构体的操作有remove, push_front, get_node等
**remove**
移除节点，更新首尾指针
```
void remove(Node *x){
        x->prev->next=x->next;
        x->next->prev=x->prev;
    }
```
**push_front**
队列头插入新节点
```
void push_front(Node *x){
        x->prev=dummy;
        x->next=dummy->next;
        x->prev->next=x;
        x->next->prev=x;
    }
```
**get_node**
模拟LRU中访问某个元素的操作，每当访问一个元素，若不存在返回空，存在则重新将它插入到头部
```
void push_front(Node *x){
        auto it=mp.find(key);
        if(it==mp.end())
            return nullptr;
        Node *t=it->second;
        remove(t);
        push_front(t);
        return t;
    }
```
定义一个哈系表`unordered_map<int,Node*>`保存每个key对应的Node，方便查询

定义一个哨兵节点dummy，`dummy->next`指向队列头，`dummy->prev`指向队列尾，初始化`dummy->next=dummy，dummy->prev=dummy`

在get时，调用get_node，如果为空则返回-1,否则就返回对应value

在put时，同样调用get_node，若已存在则修改`node->value`即可;否则使用push_front插入新节点，然后若超出容量，删除`dummy->prev`指向的队列尾元素。
### 代码
```
class Node{
public:
    int key,value;
    Node *prev,*next;
    Node(int k=0, int v=0) : key(k),value(v){}
};

class LRUCache {
private:    
    unordered_map<int,Node*> mp;
    Node *dummy;
    int sz;

    void remove(Node *x){
        x->prev->next=x->next;
        x->next->prev=x->prev;
    }

    void push_front(Node *x){
        x->prev=dummy;
        x->next=dummy->next;
        x->prev->next=x;
        x->next->prev=x;
    }

    Node *get_node(int key){
        auto it=mp.find(key);
        if(it==mp.end())
            return nullptr;
        Node *t=it->second;
        remove(t);
        push_front(t);
        return t;
    }

public:
    
    LRUCache(int capacity):sz(capacity){
        dummy=new Node();
        dummy->prev=dummy;
        dummy->next=dummy;
    }
    
    int get(int key) {
        Node *node=get_node(key);
        return node ? node->value:-1; 
    }
    
    void put(int key, int value) {
        Node *node=get_node(key);
        if(node){
            node->value=value;
            return;
        }
        node =new Node(key,value);
        mp[key]=node;
        push_front(node);
        if(mp.size()>sz){
            Node *back=dummy->prev;
            mp.erase(back->key);
            remove(back);
            delete back;
        }
            
    }
};

```
