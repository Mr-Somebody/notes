---
title: Shortest Palindrome
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-23 15:41:00
layout: post
tags: [dp, palindrome, dynamic programming]
---
Given a string S, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

For example:

Given "aacecaaa", return "aaacecaaa".

Given "abcd", return "dcbabcd".
<!--more-->

[Leetcode Link](https://leetcode.com/problems/shortest-palindrome)

## Methodology
For we can only add characters in front the string, all we need to do is to find the longest palindrome substring start from the first character of the string. After we find the palindrome, we add the reversed remaing substring to the front of the string to get the final result.

To find the longest palindrome started from the first character, we can use methods discussed in the article [Longest Palindromic Substring](/programming/2017/04/10/Longest-Palindromic-Substring.html). Here we use the manacher method which find the longest palindrome in O(n) time. The difference is that when we traverse the result table to find the longest, only the palindromes started from the first character count.

## Source Code
```C++
class Solution {
public:
    int manacher(std::string s) {
        char *tmp_s = (char *)malloc(sizeof(char) * (s.length() * 2 + 1));
        int *table = (int *)malloc(sizeof(int) * s.length() * 2 + 1);
        for (int i = 0; i < s.length() * 2 + 1; ++i) {
            if (i % 2) {
                tmp_s[i] = s[i / 2];
            } else {
                tmp_s[i] = '#';
            }
        }
        table[0] = 0;
        table[1] = 1;
        int right_most_center = 1;
        for (int i = 2; i < s.length() * 2 + 1; ++i) {
            int right = right_most_center + table[right_most_center];
            int radius = 0;
            //printf("B:%d,%d,%d\n",i, right_most_center, right);
            if (i < right) {
                int op_i = 2 * right_most_center - i;
                radius = table[op_i];
                if (i + radius > right) {
                    radius = right - i;
                }
            }
            if (radius + i < right) {
                table[i] = radius;
            } else {
                while (i + radius < s.length() * 2 + 1 &&
                       i - radius >= 0 && tmp_s[i + radius] == tmp_s[i - radius]) {
                    //printf("S:%d,%d\n", i, radius);
                    table[i] = radius;
                    ++radius;
                }
                right_most_center = i;
            }
        }
        int max_s = 0;
        int max_len = 1;
        bool mark = 1;
        for (int i = 0; i < s.length() * 2 + 1; ++i) {
            //printf("A:%d,%d\n", i, table[i]);
            int len = table[i];
            if (len > max_len) {
                max_s = i / 2 - table[i] / 2;
                if (max_s == 0) {
                    max_len = len;
                }
            }
        }
        free(table);
        free(tmp_s);
        return max_len;
    }

    std::string shortestPalindrome(std::string s) {
        int max_len = manacher(s);
        char *buf = (char*)malloc(sizeof(char) * (s.length() * 2 + 1 - max_len ));
        int pos = -1;
        for (int i = s.length() - 1; i >= max_len; --i) {
            buf[++pos] = s[i];
        }
        for (int i = 0; i < s.length(); ++i) {
            buf[++pos] = s[i];
        }
        buf[++pos] = '\0';
        std::string ret(buf);
        free(buf);
        return ret;
    }
};
```
