---
title: Increasing Triplet Subsequence
category: Programming
comments: true
excerpt_separator: <!--more-->
layout: post
---
Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.
<!--more-->
Formally the function should:
Return true if there exists i, j, k
such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.
Your algorithm should run in O(n) time complexity and O(1) space complexity.

[Leetcode Link](https://leetcode.com/problems/increasing-triplet-subsequence)

## Methodology
Intuitively, we need a array of size 2 to remember the increasing sequence we have found. Let \\(a_{0}\\) and \\(a_{1}\\) denote the increasing sequence of length 2 we have found until now. Given another element \\(e_{k}\\), if \\(e_{k}\\) is great than \\(a_{1}\\), we find a increasing triplet sequence and return true. Otherwise, if \\(e_{k}\\) is great than \\(a_{0}\\) but less than \\(a_{1}\\), all we need to do is simply replace \\(a_{1}\\) with \\(e_{k}\\) (Because for any data great than \\(a_{1}\\), it's great than \\(e_{k}\\). So this is a constrain relax step).

Then what if \\(e_{k}\\) is less than \\(a_{0}\\)? Because the coming data might in (\\(e_{k}\\), \\(a_{0}\\)), so we must also remember \\(e_{k}\\). So the array we need increases to size 3. Then what? Those three datas we remember split the integer set into four segments: \\((,e_{k}]\\), \\((e_{k}, a_{0}]\\), \\((a_{0}, a_{1}]\\) and \\((a_{1}, )\\). Let's consider those four segments case by case.
1. \\(e_{k+1}\\) falls into \\((,e_{k}]\\). \\(e_{k+1}\\) is more likely get a triplet sequence than \\(e_{k}\\), so we replace \\(e_{k}\\) with \\(e_{k+1}\\)
2. \\(e_{k+1}\\) falls into \\((e_{k}, a_{0}]\\). We get to 2-increasing sequence: \\(e_{k}\\) and \\(e_{k+1}\\), \\(a_{0}\\) and \\(a_{1}\\). Apparently the former is more likely to find a triplet increasing sequence, so we can ignore (forget/replace) the later one.
3. \\(e_{k+1}\\) falls into \\((a_{0}, a_{1}]\\). This case is similar to case 2. We get new 2-increasing sequence:  \\(e_{k}\\) and \\(e_{k+1}\\), \\(a_{0}\\) and \\(e_{k+1}\\), and we forget the later one.
4. \\(e_{k+1}\\) falls into \\((a_{1}, )\\). We find a increasing triplet sequence: \\(a_{0}\\), \\(a_{1}\\) and \\(e_{k+1}\\).

Till now, we build our solution on the assumption that we have gotten a 2-increasing sequence. Consider the case build from the scrach. If we remember nothing when \\(e_{k}\\) comes, we just remember \\(e_{k}\\) as \\(a_{0}\\). When \\(e_{k+1}\\) comes, if \\(e_{k+1}\\) is great than \\(e_{k}\\), we find a 2-increasing sequence and can solve it with method described above. What if \\(e_{k+1}\\) is less(or equal) than \\(e_{k}\\)? Simply replace \\(e_{k}\\) with \\(e_{k+1}\\) for the later one is more likely to get a triplet sequence.

Through the analysis above, we can easy get the space complexity is O(1) for we only need a array of size 3 to remeber the data we found. Because we only need to traverse all datas one time, the time complexity is linear.

## Source Code
```C++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int buf[3] = {-1, -1, -1};
        for (uint i = 0; i < nums.size(); ++i) {
            //printf("i=%d,%d,%d,%d\n", i, buf[0], buf[1], buf[2]);
            if (buf[0] == -1) {
                buf[0] = i;
            } else if (nums[i] < nums[buf[0]]) {
                if (buf[1] == -1) {
                    buf[0] = i;
                } else if (buf[2] == -1 || nums[i] < nums[buf[2]]){
                    buf[2] = i;
                } else if(nums[i] > nums[buf[2]]) {
                    buf[0] = buf[2];
                    buf[1] = i;
                    buf[2] = -1;
                }
            } else if (nums[i] == nums[buf[0]] && buf[2] != -1) {
                buf[0] = buf[2];
                buf[1] = i;
                buf[2] = -1;
            } else if (nums[i] > nums[buf[0]]) {
                if (buf[1] == -1) {
                    buf[1] = i;
                } else if (nums[i] <= nums[buf[1]]) {
                    buf[1] = i;
                    if (buf[2] != -1) {
                        buf[0] = buf[2];
                        buf[2] = -1;
                    }
                } else if (nums[i] > nums[buf[1]]) {
                    return true;
                }
            }
        }
        //printf("i=%d,%d,%d,%d\n", nums.size(), buf[0], buf[1], buf[2]);
        return false;
    }
};
```
