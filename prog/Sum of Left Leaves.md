---
title: Sum of Left Leaves
category: Programming
comments: true
---
## Description
>Find the sum of all left leaves in a given binary tree.

[Leetcode Link](https://leetcode.com/problems/sum-of-left-leaves/#/description)

## Methodology
This is a simple question. All we need to do is to traverse the tree once, during which if we meet leaf node who is the left child of its parent, we increase the final value by the node value. Here we use recursive depth first traversal. While one can use hierarchy traversal as well. 

## Source Code
```C++
class Solution {
public:
    void _recurse(TreeNode* node, bool left) {
        if (node->left) _recurse(node->left, true);
        if (node->right) _recurse(node->right, false);
        if (!(node->left) && !(node->right) && left) _sum += node->val;
    }

    int sumOfLeftLeaves(TreeNode* root) {
        _sum = 0;
        if (!root) return _sum;
        _recurse(root, false);
        return _sum;
    }
private:
    int _sum;
};
```
