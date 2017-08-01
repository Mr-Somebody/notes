---
title: Linked List Cycle II
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 14:28:00
layout: post
tags: [list, cycle]
---
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:
Can you solve it without using extra space?
<!--more-->

[Leetcode Link](https://leetcode.com/problems/linked-list-cycle-ii)

## Methodology
First, we need to determin if the list contains a circle. We can use two pointer, a slow one and a quick one, to achieve this. Both of the pointers initialized to the head of the list. However, the slow one move one step forward one turn, while the quick move two step forward. Keeping move them forward, util one of them is NULL or they meet each other. If it's the former case, it means the list does not contain a circle; Otherwise, it does.

After checking the existence of circle, if a circle does not exist in the list return NULL. Otherwise, we reset the slow pointer to the head, and move these two pointers forward together both one step per turn util they meet each other. The node they meet is where the cycle begins.

The circle checking process is intelligible. So we discuss the finding step only. Let \\[ m \\] denotes the number of nodes not in the circle, while \\[ n \\] represents the number of nodes in the circle. Suppose when the two pointers met in the first step, the slow pointer has moved \\[ t \\] steps in the circle. So we can get the slow pointer moves \\[ m+t \\] steps total, and the quick pointer moves \\[ 2*(m+t) \\]. Also we know the quick pointer moves some rounds of the circle, suppose \\[ k \\] rounds. So we can get \\[ 2*(m+t)=m+k*n \\], that is \\[ k*n - t = m \\].

Then we go to the second step, we move these two pointers equally paces. When the slower one moves \\[ m \\] steps, it reaches the des-node. At the same time, the quick pointer moves \\[ k*n - t \\] steps, which is k round of the circle and then go back \\[ t \\]. We know the quick node is at the position \\[ t \\] step forward the des-node after the first step. So, the quick pointer is also at the des-node. That's it! Both of them meet at the node we want to find.

## Source Code
```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) break;
        }
        if (fast == NULL || fast->next==NULL) return NULL;
        fast = head;
        while (fast != slow) {
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```
