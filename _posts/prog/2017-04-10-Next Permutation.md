---
title: Next Permutation
category: Programming
comments: true
excerpt_separator: <!--more-->
---
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
<!--more-->

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

[Leetcode Link](https://leetcode.com/problems/next-permutation/#/description)

## Methodology
To work out a solution, let's start with a simple problem. Suppose we are going to find next permutation of the sequence "1,2,3,4,5", which is the smallest in lexicographically order. With basic mathmatic knowledge, we can figure out permutation transformations:

"1,2,3,4,5"=>"1,2,3,5,4"=>"1,2,4,3,5"=>"1,2,4,5,3"=>"1,2,5,3,4"=>"1,2,5,4,3"=>"1,3,2,4,5"=>"1,3,2,5,4"=>"1,3,4,2,5"=>"1,3,4,5,2"=>....

Let's add one extra number "0" at both sides of the permutation, then we get a transformation "0,1,2,3,4,5,0"=> "0,1,2,3,5,4,0". The same for the remainning:

"0,1,2,3,4,5,0"=>"0,1,2,3,5,4,0"=>"0,1,2,4,3,5,0"=>"0,1,2,4,5,3,0"=>"0,1,2,5,3,4,0"=>"0,1,2,5,4,3,0"=>"0,1,3,2,4,5,0"=>..

With carefully observation, we can find out that every time we meet a descending order sequence, we exchange the last element of ascending order sequence and the smallest number bigger than it. Take "0,1,2,3,4,5,0" for instance, we get a ascending order sequence "0,1,2,3,4" and a descending order sequence "5,0", then we exchange "5" and "4". This is true for other transformations.
Besides exchange, we also need to rerange the descending order sequence into ascending order.

## Proof
Suppose the current permutation is \\(a_1,....,a_k,a_{k+1},....,a_n\\), \\(a_1,...,a_k\\) is in ascending order, \\a(a_{k+1},...,a_n\\) is in descending order, and \\(a_r\\) is the smallest number bigger than \\(a_k\\) in sequence \\(a_{k+1},...,a_n\\). Using the method descibed above, we can get next permutation: \\(a_1,....,a_{r}, b_1,...,a_k,...,b_{n-k}\\), in which the subsequence \\(b_1,...,a_k,...b_{n-k}\\) is the ascending sorted result of sequence \\(a_{k+1},...,a_{k},...,a_{n}\\) after exchange \\(a_k\\) and \\(a_r\\).

 Consider all other exchange cases:
 1. If we exchange two numbers in subsequence \\(a_1,...,a_k\\), suppose \\(a_x\\) and \\(a_y\\). Let \\(a_y\\) come after \\(a_x\\), which means \\(a_y>=a_x\\). Then \\(a_1,...,a_y,...,a_x,...,a_k\\) is always larger than \\(a_1,...,a_k+1\\).
 2. If we exchange two numbers in subsequence \\(a_{k+1},..,a_n\\), the result must be smaller than the original one for this sequence is in descending order.
 3. Thus, we can only select two numbers separately from both subsequence. Consider the ascending order subsequence, if we take a number come before \\(a_{k}\\), say \\(a_{x}\\). Either we pick smaller or bigger one than \\(a_{x}\\) in the descending order subsequence will make the new sequece bigger than  \\(a_1,....,a_{r}, b_1,...,a_k,...,b_{n-k}\\) or smaller than the original one. So we can only pick \\(a_{k}\\) from the ascending subsequence. The same for the selection of descending subsequence.
 4. After exchange, the sequece is not the smallest with \\(a_r\\) at the position of \\(a_k\\), so we need to rerange the descending sequece into ascending order to make it smallest. Here, we only need to reverse this descending sequce instead of using sorting algorithms, like quick-sort.

## Source Code
```C++
class Solution {
public:
    void nextPermutation(std::vector<int>& nums) {
        uint pos = nums.size() - 1;
        while (pos > 0) {
            if (nums[pos] > nums[pos - 1]) {
                uint ipos = pos;
                while (ipos < nums.size() && nums[ipos] > nums[pos - 1]) {
                    ++ipos;
                }
                --ipos;
                int tmp = nums[ipos];
                nums[ipos] = nums[pos - 1];
                nums[pos - 1] = tmp;
                uint s = pos;
                uint e = nums.size() - 1;
                while (s < e) {
                    tmp = nums[s];
                    nums[s] = nums[e];
                    nums[e] = tmp;
                    ++s;
                    --e;
                }
                break;
            } else {
                --pos;
            }
        }
        if (pos == 0) {
            while (pos < nums.size() / 2) {
                int tmp = nums[pos];
                nums[pos] = nums[nums.size() - 1 - pos];
                nums[nums.size() - 1 - pos] = tmp;
                ++pos;
            }
        }
    }
};
```
