---
title: First Unique Character in a String
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-23 17:10:00
layout: post
tags: [string]
---
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:

s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/first-unique-character-in-a-string)

## Methodology
Simple question. Just traverse the string once and record the first occurrent position. It the character already met, change the position value to -2. After traverse the string, traverse the record array and find the smallest non-negative value. The total run time is O(n) and the space complexity is O(1).

## Source Code
```C++
class Solution {
public:
    int firstUniqChar(std::string s) {
        int buf[26];
        for (int i = 0; i < 26; ++i) buf[i] = -1;
        for (uint i = 0; i < s.length(); ++i) {
            int pos = s[i] - 'a';
            if (buf[pos] == -1) buf[pos] = i;
            else buf[pos] = -2;
        }
        int ret = s.length();
        for (int i = 0; i < 26; ++i) {
            if (buf[i] >= 0 && buf[i] < ret) {
                ret = buf[i];
            }
            //printf("%d,", buf[i]);
        }
        return ret == s.length() ? -1 : ret;
    }
};
```
