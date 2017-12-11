---
title: Diameter of Binary Tree
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-12-11 16:00:00
layout: post
tags: [dp, palindrome, dynamic programming]
---
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example:
Given a binary tree
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
<!--more-->

[Leetcode Link](https://leetcode.com/problems/diameter-of-binary-tree)

## Methodology
Treat each node as root and find the longest path rooted with this node. Among all those paths, the longest is the path we find. The longest path in a tree, is the depth of left subtree plus depth of right subtree. So the problem is converted to calculate the depth rooted with each node. DFS can solve this elegantly.

## Source Code
```C++
class Solution {
public:

  int _recurse(TreeNode* root) {
    if (root == NULL) return 0;
    int leftMaxDepth = _recurse(root->left);
    int rightMaxDepth = _recurse(root->right);
    int len = leftMaxDepth + rightMaxDepth;
    if (len > _diameter) _diameter = len;
    return std::max(leftMaxDepth, rightMaxDepth) + 1;
  }

  int diameterOfBinaryTree(TreeNode* root) {
    _diameter = 0;
    _recurse(root);
    return _diameter;
  }

private:
  int _diameter;
};
```
