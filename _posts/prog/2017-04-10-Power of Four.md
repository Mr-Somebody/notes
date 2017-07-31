---
title: Power of Four
category: Programming
comments: true
excerpt_separator: <!--more-->
layout: post
---
Given an integer (signed 32 bits), write a function to check whether it is a power of 4.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/power-of-four/#/description)

## Methodology
If a number n is power of four, say \\(4^{k}\\), then n is power of 2, which is \\(2^{2k}\\). That is move integer 1 every two bits, denote \\(2^{2*0}\\), \\(2^{2*1}\\), etc. Observe the pattern of bits: 0001, 0100, 10000, etc, we can find out that only the bits in 0x55555555 can be set to one. And also only one bit can be set to one. So the necessary and sufficient condition for a number to be power of four is that only one bit is 1 and the bit is in 0x55555555. So it is easy to solve.

In addition to check the bit is in 0x55555555, we can also find that all the numbers mod 3 equal 1. So we can also use mod 3 instead of use 0x55555555.

## Source Code
```C++
class Solution {
public:
    bool isPowerOfFour(int num) {
        // return ((num & (num - 1)) == 0) && (num & 0x55555555) != 0;
        return ((num & (num - 1)) == 0) && num % 3 == 1;
    }
};
```
