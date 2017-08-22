---
title: Decode Ways
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-22 17:10:00
layout: post
---
A message containing letters from A-Z is being encoded to numbers using the following mapping:

'A' -> 1
'B' -> 2
...
'Z' -> 26
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message "12", it could be decoded as "AB" (1 2) or "L" (12).

The number of ways decoding "12" is 2.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/decode-ways)

## Methodology
Intuitively, we can use heuristic method to solve this problems. All we need to do is to recursively search all possible patterns, and return the total number. However, with carefully observation, we can find out that this is DP problem, wich can be solved in linear time. Let's examine the essentials of DP method step by step.
1. Assumption: Let S denotes the string to decode and N denotes the length of S. Let S[i] denotes the character at position i of S and S[i:j] denotes the substring of S from i to j (excluded).
2. Optimal Subproblems: Suppose we have getton the number of ways decoding substring S[i: N] as F(i). We can derive the the number of ways decoding substring S[i - 1: N] from F(i). There includes several cases:
  1. If S[i - 1] is a positive digit number, which is in the range of [1, 9], we can not decode this substring, which means F(i - 1)=0.
  2. If i - 1 == N - 1, it means this the last character of S. Then F(i - 1)=1 if S[i - 1] is in [1, 9]; otherwise, 0.
  3. If i - 1 < N - 2 and S[i - 1] is in the range of [1, 9]. We can choose to let S[i - 1] form a single character, then the number of ways decoding equals to S[i]. If S[i - 1] == 1 or S[i - 1] == 2, we can alternatively composite S[i - 1] and S[i] to form another character, by which the number of ways decoding equals to S[i + 1]. So, we can get F(i-1)=F(i)+F(i+1) in some conditions illustrated as above.
3. Overlapping: When we calculate F(i) and F(i + 1), we need to calculate F(i+2) twice.
4. Complexity Analyse: For each position i of the string S, we only need to calculate F(i) once in constant time. So the total time complexity is O(n). We only need F(i) and F(i+1) to calculate F(i-1), so the space complexity is also O(n).

## Source Code
```C++
class Solution {
public:
    int numDecodings(std::string s) {
        if (s.length() == 0) return 0;
        std::vector<int> buf(s.length(), 0);
        int pos = s.length() - 1;
        if (s[pos] >= '1' && s[pos] <= '9')
            buf[pos] = 1;
        else
            buf[pos] = 0;
        //printf("buf[%d]=%d\n", pos, buf[pos]);
        --pos;
        while (pos >= 0) {
            if (s[pos] < '1' || s[pos] > '9') buf[pos] = 0;
            else {
                buf[pos] = buf[pos + 1];
                if ((s[pos] == '1' && s[pos + 1] >= '0' && s[pos + 1] <= '9') ||
                    (s[pos] == '2' && s[pos + 1] >= '0' && s[pos + 1] <= '6')) {
                    if (pos < s.length() - 2)
                        buf[pos] += buf[pos + 2];
                    else
                        buf[pos] += 1;
                }
            }
            //printf("buf[%d]=%d\n", pos, buf[pos]);
            --pos;
        }
        return buf[0];
    }
};
```
