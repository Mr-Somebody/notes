---
title: Bubble Sort
category: Algorithms
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2017-08-23 10:53:00
tags: [bubble, sort, python]
---
Bubble sort is a improvement of insert sort, which uses element exchange instead of element movement.
<!--more-->

In the method of insert sort, one element may be moved several times to get to the final right position. To avoid this, bubble sort put one element into the final right position once a time. Each round, bubble sort pick the largest(smallest) element in the remaining list, and put this element to the begin(end) a the sorted list. Take largest for example, each time if the element is larger than its right adjacent, we exchange them. So the current largest element is floating up like a bubble.

This method would not reduce the time complexity of sorting, which means its time complexity is also O(n*2). 

## Source Code
This is the python realization of this method.
```Python
def bubble_sort(vlist, s, e):
    """bubble sort
    """
    if not vlist:
        return
    cur = s
    while cur < e:
        pos = e - 1
        while pos > cur:
            if vlist[pos] < vlist[pos - 1]:
                tmp = vlist[pos - 1]
                vlist[pos - 1] = vlist[pos]
                vlist[pos] = tmp
            pos -= 1
        cur += 1
```
