---
title: Rotate Array
category: Programming
comments: true
excerpt_separator: <!--more-->
layout: post
---
Rotate an array of n elements to the right by k steps.
<!--more-->

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].

Note:
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

[Leetcode Link](https://leetcode.com/problems/rotate-array/#/description)

## Methodology
### Naive Method O(nk) + O(1)
Intuitively, we can rotate the array k times, and each time just move all the elements one step. In this method, we need a temp number to keep track of next element to move. So the time complexity is O(nk) and space complexity is O(1).

### Extra Space Method O(n) + O(n)
Instead of move all elements step by step, we can move elements to the right position one-off. For the elements at position i, the right position for it after rotation is (i+k)/n. To achieve this, we need a temp extra space to store the elements. After move all elements to the temp array, we need to coty all elements back to the input vector. So, the time complexity and space complexity are both O(n).

### 3-Times Reversal O(n) + O(1)
This method use exchange tricks to avoid move elements around. First, we reverse elements from 0 to n - k - 1. Then, we reverse elements from n - k to n - 1. Finally, we reverse the whole array. After this 3 times reversal operations, we get the final result. The time complexity is O(n) and we only need constant space, thus the space complexity is O(1).

To prove this method, let's assume k < n (For k >= n, we can let assign k to be k mode n):
1. Consider the element a[n - k - 1], after rotation, it should be at position a[n - 1]. Using this method, a[n - k - 1] goes to position a[0] in the first step, and goes to position a[n - 1] in the final step. The same cases for other elements from 0 to n - k - 2.  
2. Consider the element a[n - k], after rotation, it should be at position a[0]. Using this method, a[n - k] goes to position a[n - 1] in the second step, and goes to position a[0] in the final step (swiches position with a[n - k - 1]). Also the same for elements from n - k + 1 to n - 1.

## Source Code
```C++
class Solution {
public:
    void rotate(std::vector<int>& nums, int k) {
        if (nums.size() == 0) return;
        k = k % nums.size();
        if (k == 0) return;
        int s = 0;
        int e = nums.size() - k - 1;
        while (s < e) {
            int tmp = nums[s];
            nums[s] = nums[e];
            nums[e] = tmp;
            ++s;
            --e;
        }
        s = nums.size() - k;
        e = nums.size() - 1;
        while (s < e) {
            int tmp = nums[s];
            nums[s] = nums[e];
            nums[e] = tmp;
            ++s;
            --e;
        }
        s = 0;
        e = nums.size() - 1;
        while (s < e) {
            int tmp = nums[s];
            nums[s] = nums[e];
            nums[e] = tmp;
            ++s;
            --e;
        }
    }
};
```
