---
title:  Factorial Trailing Zeroes
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 14:54:00
---
Given an integer n, return the number of trailing zeroes in n!.

Note: Your solution should be in logarithmic time complexity.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/factorial-trailing-zeroes)

## Methodology
To get a trailing zero, we need a factor of 10. To get a factor of 10, we need two factors of 2 and 5. So all we need to do is to count is numbers of 2 and 5 in n!. Because, the number of 2 must be larger than the number of 5, so we just need two count the number of 5. Two do this, we can divide n by 5, recursively util n equals zero. If we have m=n/5, this means we can get m 5s in the interval (m, n]. 

## Source Code
```C++
class Solution {
public:
    int trailingZeroes(int n) {
        int counter = 0;
        while (n > 0) {
            n = n / 5;
            counter += n;
        }
        return counter;
    }
};
```
