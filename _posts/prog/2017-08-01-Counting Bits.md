---
title: Counting Bits
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-01 17:34:00
layout: post
tags: [integer, bits]
---
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

Example:
For num = 5 you should return [0,1,1,2,1,2].

Follow up:

1. It is very easy to come up with a solution with run time O(n*sizeof(integer)). But can you do it in linear time O(n) /possibly in a single pass?
2. Space complexity should be O(n).
3. Can you do it like a boss? Do it without using any builtin function like \__builtin_popcount in c++ or in any other language.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/counting-bits)

## Methodology
Intuitively, we can calculate the bits in each number by traversing all bits in a number. For a 32-bits integer, we need 32n operations (n denotes the input number), which demonstrates O(n) time complexity. However, we can use some trick to accelerate.

Check the plus one operation. For a given integer, when add one to it, it turns all consecutive tail ones into zeros, and turn the lowest zero into one. Take 3 for example. The bits representation of 3 is *0011*, when we add one to 3, it becomes 4, which is *0100*. So we can get the number of ones in a number *k* by the number of ones in *k-1*. We can use xor operations to achieve this. First, we calculate *k ^ (k - 1)*, denotes by *x*, which get the number composed of  the tailing ones in *k - 1* and the lowest one in *k*. Then we calculate the log2 value of *x + 1* to get the number of ones in *x*, which is the number of ones vanished plus one. So, we can get the difference of ones between *k* and *k - 1* by *2 - log2(x + 1)*. Using this method, we do not need to traverse every bits of the integer, but some math operations.

## Source Code
```C++
class Solution {
public:
    std::vector<int> countBits(int num) {
        std::vector<int> result;
        result.push_back(0);
        for (uint i = 1; i <= num; ++i) {
            result.push_back(result[result.size() - 1] + 2 - (int)log2((i ^ (i - 1)) + 1));
        }
        return result;
    }
};
```
