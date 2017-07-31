---
title: Edit Distance
category: Programming
comments: true
excerpt_separator: <!--more-->
layout: post
---
Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)
<!--more-->

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character

[Leetcode Link](https://leetcode.com/problems/edit-distance)

## Methodology
This is a classical DP problem. Fist of all, let's check the essentials of DP.
1. Assumption: Let A[i, j) denotes the substring from i to j (not included) of A. Let F(i, j) denotes the edit distance of word1[0, i) and word2[0, j). Let G(i, j) means steps needed to get word1[0, i) from word2[0, j), which means F(i, j) = min{all G(i, j)}.
2. Optimal Subproblems: We can calculate G(i, j) and word2[0, j) by three paths. Firstly, we can calculate G(i - 1, j), then insert a character to the latter(or delete a character from the former). Secondly, we can calculate G(i, j - 1), then insert a character to the former (or delete a character from the later). Finally, we can calculate G(i - 1, j - 1), then use a replace operation.

Let the edit distance of word1[0, i - 1) and word2[0, j) for instance, and we can get G(i, j) = G(i - 1, j) + 1. Let G(i, j) = F(i, j), then G(i - 1, j) must be F(i - 1, j). If not, that means there is G'(i - 1, j) < G(i, j), which means we can get a smaller G(i, j) < F(i, j) constrasting with the defination of edit distance. So G(i - 1, j) must be F(i - 1, j). The same for other to paths.
3. Overlapping: From the description above, we can get the recursive formula as following:

\\[F(i, j) = min(F(i - 1, j) + 1, F(i, j - 1) + 1, F(i - 1, j - 1) + t(i - 1, j - 1))\\]
\\[t(i, j) = 0 \\quad if \\quad word1[i] == word2[j], \\quad otherwise, t(i, j) = 1\\]

From the formula above, we can get that we need perform the calculation of F(i - 1, j - 1) before F(i, j) and the same for F(i - 1, j) (need the value of F(i - 1, j - 1)). So we calculate F(i - 1, j - 1) twice in both F(i, j) and F(i - 1, j). To avoid repetitions, we can use a array to store pre-calculated values.
4. Complexity Analyse: For two given string of length m and n separately, we need to calculate all F(i, j) for i in [0, m] and j in [0, n]. So the time complexity of this DP method is O(mn). Besides, we a array store intermediate results. So the space complexity is also O(mn).

## Source Code
```C++
class Solution {
public:
    int minDistance(std::string word1, std::string word2) {
        int** buf = (int**)malloc(sizeof(int*) * (word1.length() + 1));
        for (int i = 0; i <= word1.length(); ++i) {
            buf[i] = (int*)malloc(sizeof(int) * (word2.length() + 1));
            buf[i][0] = i;
        }
        for (int i = 0; i <= word2.length(); ++i) {
            buf[0][i] = i;
        }
        for (int i = 1; i <= word1.length(); ++i) {
            for (int j = 1; j <= word2.length(); ++j) {
                buf[i][j] = buf[i - 1][j - 1] + (word1[i - 1] == word2[j - 1] ? 0 : 1);
                buf[i][j] = std::min(buf[i][j], buf[i - 1][j] + 1);
                buf[i][j] = std::min(buf[i][j], buf[i][j - 1] + 1);
                //printf("A:i=%d,j=%d,dis=%d\n", i, j, buf[i][j]);
            }
        }
        int result = buf[word1.length()][word2.length()];
        for (int i = 0; i < word1.length(); ++i) {
            free(buf[i]);
        }
        free(buf);
        return result;
    }
};
```
