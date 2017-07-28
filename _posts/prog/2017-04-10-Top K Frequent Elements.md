---
title: Top K Frequent Elements
category: Programming
comments: true
excerpt_separator: <!--more-->
---
## Description
>Given a non-empty array of integers, return the k most frequent elements.
<!--more-->
>
>For example,
>Given [1,1,1,2,2,3] and k = 2, return [1,2].

>Note:
>You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
>Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

[Leetcode Link](https://leetcode.com/problems/top-k-frequent-elements/)

## Methodology

## Source Code
```C++
class MyComparator {
public:
    bool operator() (const std::pair<int, int>& a, const std::pair<int, int>&b) {
        return a.second < b.second;
    }
};

class Solution {
public:
    std::vector<int> topKFrequent(std::vector<int>& nums, int k) {
        std::map<int, int> counter;
        std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int> >, MyComparator> q;
        std::vector<int>::iterator it = nums.begin();
        while (it != nums.end()) {
            if (counter.find(*it) == counter.end()) {
                counter[*it] = 0;
            }
            --counter[*it];
            ++it;
        }
        std::map<int, int>::iterator cit = counter.begin();
        while (cit != counter.end()) {
            if (q.size() < k) {
                q.push(std::pair<int, int>(cit->first, cit->second));
            } else if (cit->second < q.top().second){
                q.pop();
                q.push(std::pair<int, int>(cit->first, cit->second));
            }
            ++cit;
        }
        std::vector<int> result;
        result.reserve(k);
        while (!q.empty()) {
            result.insert(result.begin(), q.top().first);
            q.pop();
        }
        return result;
    }
};
```
