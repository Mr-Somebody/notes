---
title: Sliding Window Maximum
category: Programming
comments: true
date: 2017-07-28 16:23:01 +0800
excerpt_separator: <!--more-->
layout: post
---
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.
<!--more-->

For example,
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.
Therefore, return the max sliding window as [3,3,5,5,6,7].

Note:
You may assume k is always valid, ie: 1 ≤ k ≤ input array's size for non-empty array.

[Leetcode Link](https://leetcode.com/problems/sliding-window-maximum/#/description)

## Methodology


## Source Code
```C++
class Solution {
public:
    std::vector<int> maxSlidingWindow(std::vector<int>& nums, int k) {
        std::deque<int> q;
        int cur = 0;
        while (cur < k) {
            if (q.empty() || q.front() <= nums[k - cur - 1]) {
                q.push_front(nums[k - cur - 1]);
            }
            ++cur;
        }
        std::vector<int> ret;
        if (!q.empty()) ret.push_back(q.front());
        while (cur < nums.size()) {
            if (nums[cur - k] == q.front()) q.pop_front();
            while (!q.empty() && nums[cur] > q.back()) {
                q.pop_back();
            }
            q.push_back(nums[cur]);
            ret.push_back(q.front());
            ++cur;
        }
        return ret;
    }
};
```
