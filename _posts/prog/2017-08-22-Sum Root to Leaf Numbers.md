---
title: Sum Root to Leaf Numbers
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-22 17:34:00
layout: post
---
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/sum-root-to-leaf-numbers)

## Methodology
Just traverse the given binary tree in DFS method and during which pass the number of current node to its children. Once we meet a leaf node, increase the total value.

## Source Code
```C++
class Solution {
public:

    void _recurse(TreeNode* node, int cnum, int& total) {
        cnum = cnum * 10 + node->val;
        if (!node->left && !node->right) {
            total += cnum;
            return;
        }
        if (node->left) {
            _recurse(node->left, cnum, total);
        }
        if (node->right) {
            _recurse(node->right, cnum, total);
        }
    }

    int sumNumbers(TreeNode* root) {
        int total = 0;
        if (root == NULL)
            return 0;
        _recurse(root, 0, total);
        return total;
    }
```
