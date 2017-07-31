---
title: Isomorphic Strings
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 16:59:00
layout: post
---
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.

Note:
You may assume both s and t have the same length.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/isomorphic-strings)

## Methodology
This is a simple question, all we need to do is to record the character map during one traverse, and check these two characters in corresponding positions are conflict with the map. If conflict (one character maps to more than one), return false; Otherwise, true.

## Source Code
```C++
class Solution {
public:
    bool isIsomorphic(std::string s, std::string t) {
        char mark[256];
        char rmark[256];
        memset(mark, 0, 256);
        memset(rmark, 0, 256);
        for (int i = 0; i < s.length(); ++i) {
            if (mark[s[i]] == 0) mark[s[i]] = t[i];
            else if (mark[s[i]] != t[i]) return false;
            if (rmark[t[i]] == 0) rmark[t[i]] = s[i];
            else if (rmark[t[i]] != s[i]) return false;
        }
        return true;
    }
};
```
