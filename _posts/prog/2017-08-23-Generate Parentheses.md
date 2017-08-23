---
title: Generate Parentheses
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-23 16:30:00
layout: post
tags: [tree, search, recurse]
---
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

>[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
<!--more-->

[Leetcode Link](https://leetcode.com/problems/generate-parentheses)

## Methodology
Typical search problem. We can treat it as the binary tree search problem. Each node is splitted into to sub-tree nodes, which is adding a left parentheses or right parentheses to the current node. Traverse the whole tree can find all possible solutions. During the traverse, we can use some trick to tailor the tree, for example, if the remaining number of right parentheses is larger than left, it means this sub-tree is invalid and there no need to continue on this sub-tree. Because we need to traverse the whole tree(although with some trim), the total time complexity is O(2^n), in which n is number of pairs.

## Source Code
```C++
class Solution {
public:

    void _recurse(int rleft, int rright, int len, char *buf, std::vector<std::string>& result) {
        if (rright < rleft) {
            return;
        }
        if (rright == 0) {
            buf[len] = '\0';
            result.push_back(std::string(buf));
            return;
        }
        if (rleft > 0) {
            buf[len] = '(';
            _recurse(rleft - 1, rright, len + 1, buf, result);
        }
        if (rright > 0) {
            buf[len] = ')';
            _recurse(rleft, rright - 1, len + 1, buf, result);
        }
    }

    std::vector<std::string> generateParenthesis(int n) {
        std::vector<std::string> result;
        if (n <= 0) {
            return result;
        }
        char *buf = (char *)malloc(sizeof(char) * (n * 2 + 1));
        _recurse(n, n, 0, buf, result);
        return result;
    }
};
```
