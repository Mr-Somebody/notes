---
title: Remove Duplicates from Sorted Array
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-23 17:24:00
layout: post
tags: [array, duplicate]
---
Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/remove-duplicates-from-sorted-array)

## Methodology
Create two cursors, the first one keep tracking the last element not duplicated, and the other one keep moving forward. If the second one's value equals the first one's, move the second forward. Otherwise, move the first one forward and exchange the value of this two cursors, then move the second one forward. Keep move the second cursor forward util it reaches the end of the array. The final size of the array is the first one's postion plus one.

## Source Code
```C++
class Solution {
public:
    int removeDuplicates(std::vector<int>& nums) {
        if (nums.size() == 0) return 0;
        int cur = 0;
        int next = 1;
        while (next < nums.size()) {
            if (nums[next] != nums[cur]) {
                nums[++cur] = nums[next];
            }
            ++next;
        }
        nums.resize(cur + 1);
        return cur + 1;
    }
};
```
