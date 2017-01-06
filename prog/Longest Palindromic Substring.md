---
title: Longest Palindromic Substring
category: Programming
---
## Description
Todo...

## Methodology
Todo...

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
