---
title: Find All Anagrams in a String
category: Programming
comments: true
excerpt_separator: <!--more-->
---
## Description
>Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.
<!--more-->

>Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

>The order of output does not matter.

[Leetcode Link](https://leetcode.com/problems/find-all-anagrams-in-a-string/#/description)

## Methodology
The straightforward method is to check every substring whose length equals the pattern string's length to see if it's the anagram of it. For a input string of length *n* and the input pattern of length *m*, the overall time complexity is O(nm). With deep analysis, we can find out that method contains repetitions in calculations. When we Calculate the substrings (i, i + m - 1) and (i + 1, i + m), we traverse the substring (i + 1, i + m - 1) twice, which can be avoided by record the traverse result of (i + 1, i + m - 1). The method going to be discussed bellow using this idea to reduce the overall time complexity to O(n + m).

First, we count the number of characters in the pattern and use a counter array called *C*.
Second, we traverse the input string start from the left most. Let *s* denotes the start position. We move the end position *e* from right most, util C[S[e]] == 0, which means there is no more S[e] in pattern. If *e - s* equals *m*, then we find a anagram; Otherwise, we increase *s* util C[S[e]] > 0. Then we move *e* forward again util *s* moves to the end.

There is some border conditions. When we move *s* forward, we need to ensure *s* is no larger than *e*, for S[e] may not in pattern. If S[e] does not appear in pattern, *s* should move to *e + 1*.

## Source Code
```C++
class Solution {
public:
    std::vector<int> findAnagrams(std::string s, std::string p) {
        int n = p.length();
        //
        int counter[256];
        memset(counter, 0, 256 * sizeof(int));
        for (uint i = 0; i < p.length(); ++i) {
            ++counter[p[i]];
        }
        std::vector<int> result;
        int start = 0;
        int end = 0;
        while (start < s.length()) {
            printf("%d\n", start);
            while (end < s.length() && counter[s[end]] > 0) {
                --counter[s[end]];
                ++end;
            }
            if (end - start == n) {
                result.push_back(start);
            }
            while (start < end && counter[s[end]] == 0) {
                ++counter[s[start]];
                ++start;
            }
            if (counter[s[end]] == 0) {
                ++start;
                end = start;
            }
        }
        return result;
    }
};
```
## Related Problem
[Valid Anagram](/prog/Valid Anagram)
