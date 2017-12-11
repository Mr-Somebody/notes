---
title: Reverse Vowels of a String
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-12-11 17:19:00
layout: post
tags: [string]
---
Write a function that takes a string as input and reverse only the vowels of a string.

*Example 1:*
Given s = "hello", return "holle".

*Example 2:*
Given s = "leetcode", return "leotcede".

*Note:*
The vowels does not include the letter "y".
<!--more-->

[Leetcode Link](https://leetcode.com/problems/reverse-vowels-of-a-string)

## Methodology
Just an extension of string reversal. The difference is just reverse the vowels. To solve this problem, we define to pointers, one traverse from the start to end, and the other traverse reverse direction. Once both of them meet vowels, exchange the values. Do this job util the the forward pointer meet the backward pointer.

## Source Code
```C++
class Solution {
public:

  bool isVowel(char c) {
    if (c <= 'Z') c = c + 'a' - 'A';
    return c == 'a' || c == 'e' || \
      c == 'i' || c == 'o' || c == 'u';
  }

  std::string reverseVowels(std::string s) {
    char *buf = (char*)malloc(sizeof(char) * (s.length() + 1));
    memcpy(buf, s.c_str(), s.length() + 1);
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
      while (left < right && !isVowel(s[left])) ++left;
      while (left < right && !isVowel(s[right])) --right;
      if (left < right) {
	buf[left] = s[right];
	buf[right] = s[left];
      }
      ++left;
      --right;
    }
    std::string ret(buf);
    free(buf);
    return ret;
  }
};
```
