---
title: Third Maximum Number
category: Programming
comments: true
excerpt_separator: <!--more-->
---
## Description
>Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).
<!--more-->

[Leetcode Link](https://leetcode.com/problems/third-maximum-number/#/description)

## Methodology
Solution to this problem is quite straight-forward. Just find the largest, then the second largest, then the third largest. Total comparisons is (n - 1) + (n - 2) + (n - 3) = 3n - 6, which is O(n) time complexity. However, in the realization bellow, we use a vector to store current three most significant number, and traverse only one time. The time complex is also O(n) which will not gain more efficiency.

## Source Code
```C++
class Solution {
public:
    int thirdMax(std::vector<int>& nums) {
        std::vector<int> buf;
        for (int i = 0; i < nums.size(); ++i) {
            int j = 0;
            for (; j < buf.size(); ++j) {
                if (nums[i] >= buf[j]) break;
            }
            if ((j < buf.size() && buf[j] < nums[i]) || (j >= buf.size() && j < 3)) {
                buf.insert(buf.begin() + j, nums[i]);
                if (buf.size() > 3) buf.erase(buf.end() - 1);
            }
        }
        return buf.size() == 3 ? buf[2] : buf.size() > 0 ? buf[0] : 0;
    }
};
```

## Related Problem
[H-Index](/prog/H-Index)
