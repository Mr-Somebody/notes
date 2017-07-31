---
title: Reorder List
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 14:44:00
layout: post
---
Given a singly linked list L: L0->L1->…->Ln-1->Ln,
reorder it to: L0->Ln->L1->Ln-1->L2->Ln-2->…

You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/reorder-list)

## Methodology
In the example above, if we treat list {1, 2} as a list and {4, 3} as a list, then the final result is the merge of these two lists. However, these two lists both came from the same biggerlist, the former is the left half of that list and the later is the right half reversed list of that list. So all we need to do is to reverse the right half a the list, then merge these two sub lists.

## Source Code
```C++
class Solution {
public:
    void reorderList(ListNode* head) {
        int n = 0;
        ListNode* node = head;
        while (node != NULL) {
            ++n;
            node = node->next;
        }
        if (n < 3) {
            return;
        }
        int k = (n + 1) / 2;
        node = head;
        while (k > 1) {
            node = node->next;
            --k;
        }
        ListNode* pre = node->next;
        node->next = NULL;
        node = pre->next;
        pre->next = NULL;
        while (node) {
            ListNode* tmp = node->next;
            node->next = pre;
            pre = node;
            node = tmp;
        }
        node = head;
        while (pre) {
            ListNode* tmp_1 = node->next;
            ListNode* tmp_2 = pre->next;
            node->next = pre;
            pre->next = tmp_1;
            node = tmp_1;
            pre = tmp_2;
        }
    }
};
```
