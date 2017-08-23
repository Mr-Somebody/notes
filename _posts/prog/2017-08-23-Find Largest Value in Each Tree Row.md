---
title: Find Largest Value in Each Tree Row
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-23 16:42:00
layout: post
tags: [tree, largest]
---
You need to find the largest value in each row of a binary tree.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/find-largest-value-in-each-tree-row)

## Methodology
All we need to do is to traverse the tree once, during which record the largest value for each level and update if necessary. Both DFS and BFS can finish the job. Here we use DFS to calculate the results.

## Source Code
```C++
void _recurse(std::vector<int>& result, int level, TreeNode* node) {
        if (node == NULL) return;
        if (level >= result.size()) result.push_back(INT_MIN);
        result[level] = std::max(result[level], node->val);
        _recurse(result, level + 1, node->left);
        _recurse(result, level + 1, node->right);
    }

    std::vector<int> largestValues(TreeNode* root) {
        std::vector<int> result;
        _recurse(result, 0, root);
        return result;
    }
```
