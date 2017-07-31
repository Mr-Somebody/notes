---
title: Convert BST to Greater Tree
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 11:22:00
layout: post
---
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/convert-bst-to-greater-tree)

## Methodology
In a BST, for a node, the values in its right subtree are all greater than its value and the values in its left subtree are all smaller than its valule. So all we need to do is to add the total of the node's right subtree to the node's value. When we using Middle Order Traverse, the traverse order is ascending. In other words, it we reverse the left and right sub trees, the traverse is in descending order. Of course, we do not need to actually reverse the tree. We can just traverse the right subtree first and then the node and left subtree. During the traverse, we keep tracking the prenode, which is the least node greater than current. Then current node's value should be increase by the value of prenode (For we traverse the prenode first then the current node, so the prenode's final value has been calculated).

## Source Code
```C++
class Solution {
public:

    void _df(TreeNode* node, TreeNode* &pre_node) {
        if (node == NULL) return;
        _df(node->right, pre_node);
        node->val += pre_node ? pre_node->val : 0;
        pre_node = node;
        _df(node->left, pre_node);
    }

    TreeNode* convertBST(TreeNode* root) {
        TreeNode* nullNode = NULL;
        _df(root, nullNode);
        return root;
    }
};
```
