---
title: Combination Sum
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-08-01 17:59:00
layout: post
tags: [array, combination, integer, set]
---
Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:
1. All numbers (including target) will be positive integers.
2. The solution set must not contain duplicate combinations.

For example, given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[
  [7],
  [2, 2, 3]
]
<!--more-->

[Leetcode Link](https://leetcode.com/problems/combination-sum)

## Methodology
This is just a searching problem, we try all possibles and return the valid combinations. We try possible combinations using recursive method. First, we sort the array into ascending order. Then start from the left most, try put a number into the result, then decrease the target by that number, and recursive calling for that position. For numbers can be used repetively, we must jump the same number, which means we only take the first repetive number into account, others are ignored.

## Source Code
```C++
class Solution {
public:

    void _recurse(vector<vector<int> >& results, vector<int>& candidates,
                  vector<int>& cur, int pos, int target) {
        if (target == 0) {
            results.push_back(cur);
            return;
        }
        while (pos < candidates.size() && candidates[pos] <= target) {
            if (pos > 0 && candidates[pos] == candidates[pos - 1]) {
                ++pos;
                continue;
            }
            cur.push_back(candidates[pos]);
            _recurse(results, candidates, cur, pos, target - candidates[pos]);
            cur.erase(cur.end() - 1);
            ++pos;
        }
    }

    vector<vector<int> > combinationSum(vector<int>& candidates, int target) {
        std::sort(candidates.begin(), candidates.end());
        vector<vector<int> > results;
        vector<int> cur;
        _recurse(results, candidates, cur, 0, target);
        return results;
    }
};
```
