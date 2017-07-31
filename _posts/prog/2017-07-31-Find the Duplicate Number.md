---
title: Find the Duplicate Number
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 15:20:00
---
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Note:
1. You must not modify the array (assume the array is read only).
2. You must use only constant, O(1) extra space.
3. Your runtime complexity should be less than O(n2).
4. There is only one duplicate number in the array, but it could be repeated more than once.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/find-the-duplicate-number)

## Methodology
The straightforward method is using a counter of size (n + 1) and count the occurrence times of each number. However, this would cost O(n) space complexity, which can not meet the requiremnt. Consider the representation of a integer in computer, 32-bits. Instead count the occurrences of each number, we can count the bits occurrence times. If one number occurrence more than once, the bits of that number must occurrence more offen than the situation with no repeat numbers. For the bits with value of 1 are what we care about, we only need to count the numbers of ones. First, we count the occurrence of bits from 1 to n. Then we count the occurrence of bits of the array. For one bit, if the occurrences of 1 in the array is bigger than in [1, n], it means this bit of the duplicate number is 1. Iterate all 32 bits, we get the number. The total time complexity is O(n) and the extra space needed is constant, so the space complexity is O(1).

## Source Code
```C++
class Solution {
public:
    int findDuplicate(std::vector<int>& nums) {
        if (nums.size() < 2) {
            return -1;
        }
        int one[32];
        int zero[32];
        for (int i = 0; i < 32; ++i) {
            one[i] = 0;
            zero[i] = 0;
        }
        for (int i = 1; i < nums.size(); ++i) {
            for (int k = 0; k < 32; ++k) {
                int a = (i >> k) & 0x01;
                one[k] += a;
                zero[k] += 1 - a;
            }
        }
        /*
        printf("one:");
        for (int i = 0; i < 32; ++i) {
            printf("%d,", one[i]);
        }
        printf("\n");
        printf("zero:");
        for (int i = 0; i < 32; ++i) {
            printf("%d,", zero[i]);
        }
        printf("\n");
        */
        for (int i = 0; i < nums.size(); ++i) {
            for (int k = 0; k < 32; ++k) {
                int a = (nums[i] >> k) & 0x01;
                one[k] -= a;
                zero[k] -= 1 - a;
            }
        }
        int ret = 0;
        for (int i = 31; i >= 0; --i) {
            ret = (ret << 1);
            if (one[i] < 0) {
                ret = (ret | 0x1);
            }
        }
        return ret;
    }
};
```
