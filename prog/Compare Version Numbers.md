---
title: Compare Version Numbers
category: Programming
comments: true
---
## Description
>Compare two version numbers version1 and version2.
If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

>You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.
For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

>Here is an example of version numbers ordering:

>0.1 < 1.1 < 1.2 < 13.37

[Leetcode Link](https://leetcode.com/problems/compare-version-numbers/)

## Methodology
Just a series of comparisons during traverse. A border condition to remained is that if  these to version numbers are not equal lengths in dots, we need to traverse the remaining version number. If the remaining are all zeros and dots, they are the same; otherwise, the longer is larger.

## Source Code
```C++
class Solution {
public:
    int compareVersion(std::string version1, std::string version2) {
        int i = 0;
        int j = 0;
        while (i < version1.length() && j < version2.length()) {
            int a = atoi(&(version1.c_str()[i]));
            int b = atoi(&(version2.c_str()[j]));
            //printf("%d,%d\n", a, b);
            if (a > b) return 1;
            else if (a < b) return -1;
            else {
                while (version1[i] != '.' && i < version1.length()) ++i;
                ++i;
                while (version2[j] != '.' && j < version2.length()) ++j;
                ++j;
            }
            //printf("i=%d,j=%d\n", i, j);
        }
        while (i < version1.length() && (version1[i] == '0' || version1[i] == '.')) ++i;
        while (j < version2.length() && (version2[j] == '0' || version2[j] == '.')) ++j;
        if (i >= version1.length() && j >= version2.length()) return 0;
        else if (i >= version1.length()) return -1;
        else return 1;
    }
};
```
