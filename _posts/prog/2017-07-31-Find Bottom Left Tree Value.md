---
title: Find Bottom Left Tree Value
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 17:08:00
layout: post
---
Given a binary tree, find the leftmost value in the last row of the tree
<!--more-->

[Leetcode Link](https://leetcode.com/problems/find-bottom-left-tree-value)

## Methodology
This problem can be solved in one path traverse, and both depth and level order can achieve this. During the travers, we track the maximum depth we meet and use a value A record the leftmost value of current maximum depth. If we meet a node with larger depth, we replace A with current value, and update maximum depth. After we traverse the whole, we get the result value.

## Source Code
```C++
class Solution {
public:

    void _recurse(TreeNode* node, int depth) {
        if (node == NULL) return;
        if (depth > maxDepth) {
            maxDepth = depth;
            bottomLeftValue = node->val;
        }
        _recurse(node->left, depth + 1);
        _recurse(node->right, depth + 1);
    }

    int findBottomLeftValue(TreeNode* root) {
        maxDepth = -1;
        _recurse(root, 0);
        return bottomLeftValue;
    }

private:
    int maxDepth;
    int bottomLeftValue;
};
```
