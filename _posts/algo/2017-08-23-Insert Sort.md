---
title: Insert Sort
category: Algorithms
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2017-08-23 10:13:00
tags: [insert, sort, python]
---
Insert Sort is naive sorting method which can sort a list in O(n^2) time.
<!--more-->

It maintains a sorted list and each time meets a new element, it tries to insert the element into the right position. It can finish the job with O(n) space complexity or in-place. Besides, this method can be ether stable or not, which depends on the realization. If we find the position from end to front, it's stable; Otherwise, not.

There's some trick to reduce the total run time of this method, like using binary search in finding the right position to insert the element. However, after find the right position, we also need to move all elements behind that position. So this trick would not decrease the time complexity.

## Source Code
This is the python realization of this method.
```Python
def insert_sort(vlist, s, e):
    """insert sort
    """
    if not vlist:
        return
    cur = s + 1
    while cur < e:
        v = vlist[cur]
        pos = cur - 1
        while pos >= s and vlist[pos] > v:
            vlist[pos + 1] = vlist[pos]
            pos -= 1
        vlist[pos + 1] = v
        cur += 1
```
