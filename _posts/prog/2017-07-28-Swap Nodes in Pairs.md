---
title: Swap Nodes in Pairs
category: Programming
comments: true
date: 2017-07-28 16:23:01 +0800
excerpt_separator: <!--more-->
---
Given a linked list, swap every two adjacent nodes and return its head.
<!--more-->

For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.

Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.

[Leetcode Link](https://leetcode.com/problems/swap-nodes-in-pairs/#/description)

## Methodology


## Source Code
```C++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (head == NULL) return head;
        int i = 1;
        ListNode *pre = NULL, *cnode = head, *tnode = NULL, *next_tnode = NULL, *nhead = head, *tmp;
        if (head->next != NULL) {
            nhead = head->next;
        }
        while (cnode != NULL) {
            if (i % 2) {
                tnode = next_tnode;
                next_tnode = cnode;
                tmp = cnode->next;
                cnode->next = NULL;
                cnode = tmp;
            } else {
                if (tnode != NULL)
                    tnode->next = cnode;
                pre = cnode;
                cnode = cnode->next;
                pre->next = next_tnode;
            }
            ++i;
        }
        if (i % 2 == 0 && tnode != NULL) tnode->next = next_tnode;
        return nhead;
    }
};
```
