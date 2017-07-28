---
title: Bulls and Cows
category: Programming
comments: true
excerpt_separator: <!--more-->
---
You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.
<!--more-->

For example:

Secret number:  "1807"
Friend's guess: "7810"
Hint: 1 bull and 3 cows. (The bull is 8, the cows are 0, 1 and 7.)
Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. In the above example, your function should return "1A3B".

Please note that both secret number and friend's guess may contain duplicate digits, for example:

Secret number:  "1123"
Friend's guess: "0111"
In this case, the 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow, and your function should return "1A1B".
You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

[Leetcode Link](https://leetcode.com/problems/bulls-and-cows)

## Methodology
This a simple problem, here just explain the coding bellowing. To get bulls (represented by A) and cows (represented by B), we need two times traverse. First time we count the number of A, and count the number a remaining numbers in secret (not bulls). Then we traverse again, two see the number both in guess and counter but not a bulls. Then we get cows.

## Source Code
```C++
class Solution {
public:
    std::string getHint(std::string secret, std::string guess) {
        int counter[10];
        int A = 0;
        int B = 0;
        memset(counter, 0, sizeof(int) * 10);
        for (int i = 0; i < secret.length(); ++i) {
            if (secret[i] == guess[i]) {
                ++A;
            } else {
                ++counter[secret[i] - '0'];
            }
        }
        for (int i = 0; i < secret.length(); ++i) {
            if (secret[i] == guess[i]) {
                continue;
            } else if (counter[guess[i] - '0'] > 0){
                --counter[guess[i] - '0'];
                ++B;
            }
        }
        char buf[1024];
        snprintf(buf, 1024, "%dA%dB", A, B);
        return std::string(buf);
    }
};
```
