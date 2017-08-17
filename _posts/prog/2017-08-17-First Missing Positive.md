---
title: First Missing Positive
category: Programming
comments: true
excerpt_separator: <!--more-->
layout: post
---
Given an unsorted integer array, find the first missing positive integer.

For example,
Given [1,2,0] return 3,
and [3,4,-1,1] return 2.

Your algorithm should run in O(n) time and uses constant space.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/first-missing-positive)

## Methodology
The trick is to put every positive number into the right position, at where A[pos] = pos. Then traverse the array again to find the first missing integer. 

## Source Code
```C++
class Solution {
public:
    int firstMissingPositive(std::vector<int>& nums) {
        if (nums.size() == 0) {
            return 1;
        }
        int pos = 0;
        while (pos < nums.size()) {
            if (nums[pos] > 0) {
                int tpos = pos;
                while (nums[tpos] <= nums.size() && nums[nums[tpos] - 1] != nums[tpos]) {
                    int tmp = nums[nums[tpos] - 1];
                    nums[nums[tpos] - 1] = nums[tpos];
                    nums[tpos] = tmp;
                }
            }
            ++pos;
        }
        //printVector(nums);
        int exp = 1;
        pos = 0;
        while (pos < nums.size() && nums[pos] == exp) {
            ++pos;
            ++exp;
        }
        return exp;
    }
};
```
