---
title: k个一组翻转链表
date: 2024-12-24 10:24:58
categories: 机试
math:
tags:
---
## k个一组翻转链表

<!-- TOC -->

- [k个一组翻转链表](#k个一组翻转链表)
    - [题目描述](#题目描述)
    - [思路](#思路)
    - [代码](#代码)

<!-- /TOC -->

[原题](https://leetcode.cn/problems/reverse-nodes-in-k-group/description)


### 题目描述
给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

### 思路
这道题之前也做过，不过用的是辅助数组的方法。这次用在原链表上进行操作。
主要就是先确定一个长为k的区间，然后进行翻转，返回翻转后的头节点和尾节点。
然后在主函数中进行遍历，每次先找一段长为k的区间，记录这个区间的头节点和尾节点，然后进行翻转，得到新的头节点和尾节点，然后将这个区间的头节点和尾节点连接到原链表上。

这里顺便记录一下翻转某个区间并返回新的头节点和尾节点的模板：
```
pair<ListNode*,ListNode*> reverse(ListNode* head, ListNode*tail){
    ListNode* pre=tail->next;
    ListNode* l=head;
    while(pre!=tail){
        ListNode* t=l->next;
        l->next=pre;
        pre=l;
        l=t;
    }
    return {pre,head};
}

```

### 代码
```
class Solution {
public:
    pair<ListNode*,ListNode*> reverse(ListNode* head, ListNode*tail){
        ListNode* pre=tail->next;
        ListNode* l=head;
        while(pre!=tail){
            ListNode* t=l->next;
            l->next=pre;
            pre=l;
            l=t;
        }
        return {pre,head};
    }
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy=new ListNode(0);
        dummy->next=head;
        ListNode* pre=dummy;
        while(head){
            ListNode *tail=pre;
            for(int i=0;i<k;++i){
                tail=tail->next;
                if(tail==nullptr)
                    return dummy->next;
            }
            ListNode* t=tail->next;
            pair<ListNode*,ListNode*> res=reverse(head,tail);
            head=res.first;
            tail=res.second;
            pre->next=head;
            tail->next=t;
            pre=tail;
            head=t;
        }
        return dummy->next;
    }
};
```