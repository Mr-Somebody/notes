---
title: Rotate List
category: Programming
---
## Description
>Given a list, rotate the list to the right by k places, where k is non-negative.
>
>For example:
>Given 1->2->3->4->5->NULL and k = 2,
>return 4->5->1->2->3->NULL.

[Leetcode Link](https://leetcode.com/problems/rotate-list/)

## Methodology

### O(n) Method

#### Brief
Rotate a list by *k* means mv tail *k* elements front. The straight-forward method is rotate the list *k* times and each time move the last element first. The time complex of straight-forward method is O(n^2), which is inefficient.

More efficient method is to move tail *k* elements together, which is method introduced below.

#### Steps
1. Count the length of list as *N*.
2. Mod *k* by *N*:　*k = k % N*.
3. Create a pointer *pre* and move it *N - k - 1* steps, then the new head of the rotated list is *pre->next* and assign *pre->next* to a new pointer *nhead = pre->next*.
4. Create another pointer *node = pre->next* and move it to the last node.
5. Set *node->next = pre* and set *pre->next=NULL*.
6. Return new head *nhead*.
