---
title: Longest Palindromic Substring
category: Programming
---
## Description
Given a string s, find the longest palindome substring in s.

## Methodology
### Dynamic Programming: O(\\(n^{2}\\))
1. Methodology: Iterator all substring, find the longest substring that is a palindome.
2. Optimal Subproblems: Assume \\(a_{i,j}\\) denotes whether the substring of s from i to j (include i and j) is a palidome. If it is, \\(a_{i,j}=1\\); otherwise, 0. We can get the recursive function:

\\[a_{i,j}=1 \\quad\\quad If i==j\\]
\\[a_{i,j}=a_{i+1,j-1} \\quad\\quad If s[i]==s[j]\\]
\\[a_{i,j}=0 \\quad\\quad If s[i]!=s[j]\\]

3. Overlapping: As shown in the formula above, we might need to sovle \\(a_{i+1,j-1}\\) repeatly when calculate \\(a_{i,j}\\) \\(a_{i+1,j-1}\\). So we can use a memo to memoize \\(a_{i+1,j-1}\\) to avoid repeat calculations.
4. Prof of time complexity: The total number substring of s is O(\\(n^{2}\\)), so the time complexity of DP is O(\\(n^{2}\\))

### Manacher: O(n)
1. Intuition: Assume the longest palindome centered at *i* is \\(2 \\times R_{i}\\), where \\(R_{i}\\) is the radius of palindome. For center *i+k* if \\(R_{i} \\ge k\\), we can use the information of opposite center of *i+k* relative to *i* to calculate *i+k*'s radius.
2. Methodology: Assume \\(C_{i}\\) is center of a palindome which reaches the rightmost(Which means that \\(C_{i}+R_{i}\\) is biggest in all known palindome).
    1. For the center \\(C_{k}\\) to consider, If \\(C_{i}+R_{i} \\gt C_{k}\\), we take a look at the opposite center of \\(C_{k}\\) relative to \\(C_{i}\\) represented by \\(\\hat{C}_{k}\\) and set \\(R_{k}\\) as \\min(\\hat{R}_{k}, C_{i}+R_{i}-C_{k})\\).
    2. If the radius of  \\(C_{k}\\) reaches the rightmost of  \\(C_{i}\\). We need to look forward to see if there is more charaters for center  \\(C_{k}\\).
3. Prof of time complexity:
    1. Because we do not necessarily to update rightmost center during traverse the string. Suppose there is *k* rightmost centers. For the traverse steps that do not update rightmost center, the time complexity is O(1) and the total time complexity of which is up to O(n), so we can ignore these steps.
    2. When we move from \\(C_{i}\\) to \\(C_{i+1}\\), first we need to move forward \\(C_{i+1}-C_{i}\\) steps forward. Then to calculate \\(R_{i+1}\\) we need \\(R_{i+1}-(R_{i}+C_{i}-C_{i+1})\\) forward traversal steps. So the total number of operations move from \\(C_{i}\\) to \\(C_{i+1}\\) is

    \\[C_{i+1}-C_{i} + R_{i+1}-(R_{i}+C_{i}-C_{i+1}\\]
    \\[ \\eq R_{i+1}-R_{i}+2(C_{i+1}-C_{i})\\]

    Sum all *k-1* move operations

    \\[R_{2}-R_{1}+2(C_{2}-C_{1})\\]
    \\[R_{3}-R_{2}+2(C_{3}-C_{2})\\]
    \\[.....\\]
    \\[R_{k}-R_{k-1}+2(C_{k}-C_{k-1})\\]

    The total number of operations is

    \\[R_{k}-R_{1}+2(C_{k}-C_{1})\\]
    \\[ \\eq R_{k}+2C_{k}-(R_{1}+2C_{1})\\]
    \\[ \\le R_{k}+2C_{k} \\quad\\quad for R_{1}+2C_{1} \\ge 0\\]
    \\[ \\le 2C_{k} + N-C_{k}  \\quad\\quad for R_{k} \\le n-C_{k}\\]
    \\[ \\le 2n \\quad\\quad for C_{k} \\le n\\]

## Source Code
```C++
class Solution {
public:
    std::string longestPalindrome(std::string s) {
        //return DP(s);
        return manacher(s);
    }

    std::string DP(std::string s) {
        if (s.length() == 0) {
            return s;
        }
        int x = 0;
        int y = 0;
        int **table = (int **)malloc(sizeof(int *) * s.length());
        for (int i = 0; i < s.length(); ++i) {
            table[i] = (int *)malloc(sizeof(int) * s.length());
        }
        //
        for (int i = s.length() - 1; i >= 0; --i) {
            for (int j = i; j < s.length(); ++j) {
                if (i == j) table[i][j] = 1;
                else {
                    if (j - i > 1) {
                        table[i][j] = s[i] == s[j] ? table[i + 1][j - 1] : 0;
                    } else {
                        table[i][j] = s[i] == s[j] ? 1 : 0;
                    }
                }
                if (table[i][j] == 1 && j - i > y - x) {
                    y = j;
                    x = i;
                }
            }
        }
        //
        for (int i = 0; i < s.length(); ++i) {
            free(table[i]);
        }
        free(table);
        return s.substr(x, y - x + 1);
    }

    std::string manacher(std::string s) {
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
            printf("A:%d,%d\n", i, table[i]);
            int len = table[i];
            if (len > max_len) {
                max_s = i / 2 - table[i] / 2;
                max_len = len;
            }
        }
        free(table);
        free(tmp_s);
        return s.substr(max_s, max_len);
    }
};
```
