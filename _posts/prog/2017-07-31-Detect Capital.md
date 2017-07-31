---
title: Detect Capitals
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 10:57:00
---
Given a word, you need to judge whether the usage of capitals in it is right or not.

We define the usage of capitals in a word to be right when one of the following cases holds:

1. All letters in this word are capitals, like "USA".
2. All letters in this word are not capitals, like "leetcode".
3. Only the first letter in this word is capital if it has more than one letter, like "Google".

Otherwise, we define that this word doesn't use capitals in a right way.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/detect-capital)

## Methodology
This is automata problem. Thus all we need to do is to build a automata and these cases described above define the state trasfromations. In the method below, we deinf a mask to record the next correct state expected, then check if the next character is what we excepted. The mask is a two bits integer. If the lower bit is one, it means the next character can be a uppercase letter. If the higher bit is one, it means the next character can be a lowercase. We use bit operation & to check if the next state is right.

## Source Code
```C++
class Solution {
public:
    bool detectCapitalUse(std::string word) {
        int mask = 0x11;
        int cur = 0;
        while (cur < word.length()) {
            char c = word[cur];
            int code = (c >= 'A' && c <= 'Z') ? 0x01 : (c >= 'a' && c <= 'z') ? 0x10 : 0x00;
            printf("cur=%d, c=%c, code=%d\n", cur, c, code);
            if (!(code & mask)) return false;
            mask = cur == 0 ? (code == 0x10 ? 0x10 : 0x11) : (code == 0x01 ? 0x01 : 0x10);
            ++cur;
        }
        return true;
    }
};
```
