---
title: Binary Tree Right Side View
category: Programming
comments: true
---
## Description
>Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

[Leetcode Link](https://leetcode.com/problems/binary-tree-right-side-view/)

## Methodology
The right side view of the tree consists of the rightest nodes of each level. Thus, we can use level order traverse to find out all the rightmost nodes of the tree. Also, we can use first-order traverse as well, we record the last node we meet of each level, and the final result is the right side view.

## Source Code
```C++
typedef struct _QElem {
    TreeNode* node;
    int level;
    _QElem(TreeNode* _node, int _level) {
        node = _node;
        level = _level;
    }
}QElem;

class Solution {
public:
    std::vector<int> rightSideView(TreeNode* root) {
        std::vector<int> result;
        if (root != NULL) {
            std::queue<QElem> nq;
            nq.push(QElem(root, 0));
            result.push_back(root->val);
            int curLevel = 0;
            while (!nq.empty()) {
                QElem elem = nq.front();
                if (elem.level != curLevel) {
                    result.push_back(elem.node->val);
                    curLevel = elem.level;
                }
                if (elem.node->right) {
                    nq.push(QElem(elem.node->right, elem.level + 1));
                }
                if (elem.node->left) {
                    nq.push(QElem(elem.node->left, elem.level + 1));
                }
                nq.pop();
            }
        }
        return result;
    }
};
```
