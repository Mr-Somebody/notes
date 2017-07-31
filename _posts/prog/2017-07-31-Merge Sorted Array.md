---
title: Merge Sorted Array
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 15:11:00
---
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/merge-sorted-array)

## Methodology
Intuitive method is to create another temp array, then traverse these two arrays together, and put the smaller element into the temp array. After that we copy the elements in temp array back to the first array. This will use extra O(n + m) spaces (m and n is size of these two arrays repectively).

However, this problem assume that the size of the first array is big enough to hold all the elements, so we do not actually need to create an extra temp array. Instead we can put the elements into the first array. For the original elements in the first array occupy the first m positions, we can merge these two arrays from right to left, which means we put the largger element into the first array' final right position. The overall time complexity of this method is O(n + m) and no extra space is used (O(1) space complexity).

## Source Code
```C++
class Solution {
public:
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n) {
        int pos = m + n;
        int i = m - 1;
        int j = n - 1;
        while (i >= 0 && j >= 0) {
            if (nums1[i] >= nums2[j]) {
                nums1[--pos] = nums1[i--];
            } else {
                nums1[--pos] = nums2[j--];
            }
        }
        while (i >= 0) {
            nums1[--pos] = nums1[i--];
        }
        while (j >= 0) {
            nums1[--pos] = nums2[j--];
        }
    }
};
```
