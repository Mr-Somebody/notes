---
title: Container With Most Water
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 17:43:00
layout: post
---
Todo..........
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/container-with-most-water)

## Methodology

## Source Code
```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int s = 0;
        int e = height.size() - 1;
        int max_v = min(height[s], height[e]) * (e - s);
        while (s < e) {
            if (height[s] < height[e]) {
                while (s <= e && height[s] <= height[e]) {
                    ++s;
                    int c_v = min(height[s], height[e]) * (e - s);
                    if (c_v > max_v) {
                        max_v = c_v;
                    }
                }
            } else {
                while (s <= e && height[s] >= height[e]) {
                    --e;
                    int c_v = min(height[s], height[e]) * (e - s);
                    if (c_v > max_v) {
                        max_v = c_v;
                    }
                }
            }
        }
        return max_v;
    }
};
```
