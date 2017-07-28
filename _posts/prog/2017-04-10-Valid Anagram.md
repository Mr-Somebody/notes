---
title: Valid Anagram
category: Programming
comments: true
excerpt_separator: <!--more-->
---
Given two strings s and t, write a function to determine if t is an anagram of s.
<!--more-->

For example,
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.

Note:
You may assume the string contains only lowercase alphabets.

>Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

[Leetcode Link](https://leetcode.com/problems/valid-anagram/#/description)

## Methodology
This is simple problem which involves scarce tricks. All we need to do is to count the number of each character of string one, than reduce the couter of each character while traversing string two util we meet a character whose count is zero or reach the end of string two. If we meet a character wich zero count, it means that string one contains a extra of this character. Otherwise, they are mutual anagrams. To be sure checking the lengths of both string are equal.

## Source Code
```C++
class Solution {
public:
    bool isAnagram(std::string s, std::string t) {
        if (s.length() != t.length()) return false;
        int counter[26];
        memset(counter, 0, 26 * sizeof(int));
        for (int i = 0; i < s.length(); ++i) {
            ++counter[s[i] - 'a'];
        }
        for (int i = 0; i < t.length(); ++i) {
            int idx = t[i] - 'a';
            --counter[idx];
            if (counter[idx] < 0) return false;
        }
        return true;
    }
};
```

## Related Problems
 [Find All Anagrams in a String](/prog/Find All Anagrams in a String)
