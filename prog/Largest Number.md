---
title: Largest Number
category: Programming
comments: true
---
## Description
>Given a list of non negative integers, arrange them such that they form the largest number.

>For example, given [3, 30, 34, 5, 9], the largest formed number is 9534330.

>Note: The result may be very large, so you need to return a string instead of an integer.

[Leetcode Link](https://leetcode.com/problems/largest-number/#/description)

## Methodology
Naive method to this question is to create all possible combinations then compare them all to find the largest number. For a list contains N distinct numbers, the numbers can create is N!, which is extreme large.

Let's consider the simplest problem whic contains only two number: \\(N_1\\) and \\(N_2\\). If \\(N_1 > N_2\\) in string comparison, we can be sure that the number \\(N_1N_2\\) must larger than \\(N_2N_1\\). We can extend this to three numbers, which s also true. As you can see, we are making greedy choice each step, which is choosing the maximum number in string comparison. Using this greedy method, we just to need sort the number list in descending order then merge them together to form the largest number. The overall time complex of this greedy method is O(nlogn) (depends on the sorting algorithm).

To prove the greedy algorithm's correctness, we describe the essentials of "Greedy Algorithm" as follows:
1. Optimal Structure: Suppose the sequence \\(N_1, N_2, N_3, ....\\) is the largest combination. If we take away the first number \\(N_1\\), the remaining sequence also is the largest combination without \\(N_1\\). For if it is not, Then we can get a larger combination than \\(N_1, N_2, N_3, ...\\), which conflicts with the original largest combination assumption.
2. Greedy Choice Validity: Each time, we make a greedy choice picking the largest number in string comparison. Suppose \\(N_1\\) is the largest number in string comparison and the remaining largest combination is \\(R_1\\). If we pick another number \\(N_k\\) which is definately small than \\(N_1\\) in string comparison wich remaining largest combination \\(R_k\\). If \\(N_1\\) and \\(N_k\\) are same in length, we can make out that \\(N_1R_1\\) must be larger than \\(N_kR_k\\). If \\(N_1\\) and \\(N_k\\) are not equal in length, no matter which is short, \\(N_1R_1\\) must come before \\(N_kR_k\\) in string sorting, which means the former is larger than the later. So the greedy choice is always safe.

## Source Code
```C++
class Solution {
public:

    std::string int2str(int num) {
        if (num == 0) return std::string("0");
        int pos = 0;
        while (num > 0) {
            buf[pos++] = '0' + num % 10;
            num /= 10;
        }
        buf[pos] = '\0';
        --pos;
        int s = 0;
        while (pos > s) {
            char c = buf[pos];
            buf[pos] = buf[s];
            buf[s] = c;
            --pos;
            ++s;
        }
        return std::string(buf);
    }

    static int cmp(std::string str1, std::string str2) {
        return str1 + str2 > str2 + str1;
    }

    std::string largestNumber(std::vector<int>& nums) {
        std::vector<std::string> strs;
        strs.reserve(nums.size());
        int len = 0;
        for (uint i = 0; i < nums.size(); ++i) {
            strs.push_back(int2str(nums[i]));
            len += strs[strs.size() - 1].length();
        }
        std::sort(strs.begin(), strs.end(), cmp);
        char* buf = (char* )malloc(sizeof(char) * len + 2);
        char* p = buf;
        for (uint i = 0; i < strs.size(); ++i) {
            snprintf(p, strs[i].length() + 1, "%s", strs[i].c_str());
            p += strs[i].length();
        }
        p = buf;
        while (*p == '0' && *(p + 1) != '\0') ++p;
        std::string ret(p);
        free(buf);
        return ret;
    }

private:
    char buf[64];
};
```
