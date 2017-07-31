---
title: Intersection of Two Linked Lists
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 14:24:00
---
Write a program to find the node at which the intersection of two singly linked lists begins.

Notes:

1. If the two linked lists have no intersection at all, return null.
2. The linked lists must retain their original structure after the function returns.
3. You may assume there are no cycles anywhere in the entire linked structure.
4. Your code should preferably run in O(n) time and use only O(1) memory.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/intersection-of-two-linked-lists)

## Methodology
Suppose these two lists are ListA with length LenA and ListB with length LenB. Suppose LenA equals LenB, then we traverse these two lists together, then they will meet at the intersection node. However, They may be equal not in lengths, suppose LenA > lenB. In this case, we can move ListA (LenA-LenB) step forward, then move them together (The same case for LenB > lenA). If they have no intersection node, the shorter list the end during the traverse.

Another way to solve this problem is utilize the tech of list cycle check. We can make the tail of ListA point to the head point to the head of ListA. Then the problem is converted to check if ListB contains a cycle and find the start node of the cycle. Just remember to remove the pointer from tail to head of ListA after the node found.

## Source Code
```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lenA = 0;
        int lenB = 0;
        ListNode* node = headA;
        while (node != NULL) {
            ++lenA;
            node = node->next;
        }
        node = headB;
        while (node != NULL) {
            ++lenB;
            node = node->next;
        }
        ListNode* nodeA = headA;
        ListNode* nodeB = headB;
        if (lenA > lenB) {
            int diff = lenA - lenB;
            while (diff > 0) {
                nodeA = nodeA->next;
                --diff;
            }
        } else {
            int diff = lenB - lenA;
            while (diff > 0) {
                nodeB = nodeB->next;
                --diff;
            }
        }
        while (nodeA && nodeB) {
            if (nodeA == nodeB) {
                return nodeA;
            } else {
                nodeA = nodeA->next;
                nodeB = nodeB->next;
            }
        }
        return NULL;
    }
};
```
