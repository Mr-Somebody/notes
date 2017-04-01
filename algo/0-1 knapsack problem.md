---
title: 0-1 knapsack problem
category: Algorithms
---
## Problem
>The knapsack problem or rucksack problem is a problem in combinatorial optimization: Given a set of items, each with a weight and a value, determine the number of each item to include in a collection so that the total weight is less than or equal to a given limit and the total value is as large as possible. It derives its name from the problem faced by someone who is constrained by a fixed-size knapsack and must fill it with the most valuable items.

To say *fixed-size knapsack* or *0-1 knapsack problem*, it means the item can either put into the knapsack or not. The item can not be partially put into the knapsack (Likely put 1/3 of the item input the knapsack). To illustrate the problem more intuitively, we can treat it equal to the problem dscribed below:

There are kinds of candies put on the desk, which can sold for moneys. Each candy is associated with a defined value, which means can sold for distinct money. A child is allowed to come with a pocket to take some candies for selling. Which candies should he takes in order to sell as much money as possible.

## Methodology
From the intuition view, this is a optimization problem, which we can try DP method on it. Our aim is to maximize the total money can sell in the pocket without exceed the capacity (maximum weight) of the pocket. We assign a weight *w* and a money it can sell *v* to each candy. Also the pocket can hold at most *W* weight candies. For a candy *i* (Suppose we line up the candies from 0 to n), the weight and money of it is represented by \\(w_{i}\\) and \\(v_{i}\\) respectively. Let \\(F(k,w)\\) denotes the money we can get with the candies from 0 to k within total weight of w. Which means we can take arbitrary candies in [0,k] as long as their total weight is not bigger than w. Then out goal is to maximize \\(F(k,w)\\).

For a defined candy \\(c_{k}\\), we need to decide whether to put it into the pocket or not. Then we divided the problem \\(maximize \\quad F(k,w) \\) to 2 sub-problems: \\(maximize \\quad F(k-1, w)\\) (If we put the candy into pocket) and \\(maximize \\quad F(k-1, w-w_{k})\\). And both sub-problems are optimization problem identical to the original problem (except the scale decrease by 1). The origin probem then can be solved by the following recursive formula:

\\[maximize \\quad F(k,w))=max(maximize \\quad F(k-1,w), \\quad maximize \\quad F(k-1,w-w_{k})+v_{k})\\]

With the formula above, we can coding our recursive solution to the problem. However, we can simply find that there are overlaping sub-problems. Suppose we have three candies(0,1,2) with weight 30, 20, 20 and pocket capacity 50. When we take candy 1 and do not take candy 2, we need to calculate \\(F(0,30)\\). On the cantrary, if we take candy 2 instead of candy 1, we also need toe calulate \\(F(0,30)\\). We caculate the sub-problems \\(F(0,30)\\) twice. To sovle this, we can simply add add memoization to our recursive solutions. If \\(F(k,w)\\) is in our memo, we do not recalculate it again, but simply return the value in memo.

Two essential components of DP formulated: Optimal substructure and overlaping subproblems, we can solve the problem with DP method in polynomial time.

## Complexity analysis
We have talked about the recursive solution to this problem. Like other DP problems, we can translate the solution to bottom-up non-recursive solution. It's obvious that all we need to do is to calculate all \\(F(k,w)\\) for each possible k and w. So the time complexity of solution to this problem is O(Wn), where W is the capacity of pocket and n is the total number of candies. Apparently, we don't need to calculate \\(F(k,w)\\) for every possible *w*. we only need some of them. So when capacity of pocket is extreme large, the time complexity of bottom-up method is unacceptable. However, the memoized recursive method only calculate the desired \\(F(k,w)\\). So for large pocket capacity, the bottom-up method is losing efficiency.

## Codes
```C++
#include <stdio.h>
#include <vector>
#include <algorithm>
#include <map>

typedef struct _Knapsack {
    int weight;
    int value;
    _Knapsack(int w, int v) {
        weight = w;
        value = v;
    }
}Knapsack;

int max_value_bottom_up(std::vector<Knapsack>& input, std::vector<Knapsack>& result,
                         int max_weight) {
    int **value = (int**)malloc(sizeof(int*) * input.size());
    int **mark = (int**)malloc(sizeof(int*) * input.size());
    for (uint i = 0; i < input.size(); ++i) {
        value[i] = (int*)malloc(sizeof(int) * (max_weight + 1));
        mark[i] = (int*)malloc(sizeof(int) * (max_weight + 1));
    }
    for (int r = max_weight; r >= 0; --r) {
        value[0][r] = r >= input[0].weight ? input[0].value : 0;
        mark[0][r] = r >= input[0].weight ? 1 : 0;
    }
    for (uint i = 1; i < input.size(); ++i) {
        for (int r = max_weight; r >= 0; --r) {
            int add = -1;
            if (r >= input[i].weight) {
                add = input[i].value + value[i - 1][r - input[i].weight];
            }
            int not_add = value[i - 1][r];
            if (add > not_add) {
                value[i][r] = add;
                mark[i][r] = 1;
            } else {
                value[i][r] = not_add;
                mark[i][r] = 0;
            }
        }
    }
    result.clear();
    int w = max_weight;
    for (int i = (int)input.size() - 1; i >= 0; --i) {
        if (mark[i][w]) {
            w -= input[i].weight;
            result.push_back(input[i]);
        }
    }
    for (uint i = 0; i < input.size(); ++i) {
        free(value[i]);
        free(mark[i]);
    }
    free(value);
    free(mark);
    return value[input.size() - 1][max_weight];
}

int _recursive(std::vector<Knapsack>& input, int k, int w,
               std::vector<std::map<int, int> >&memo,
               std::vector<std::map<int, int> >& mark) {
    if (k < 0) {
        return 0;
    }
    if (memo[k].find(w) != memo[k].end()) {
        return memo[k][w];
    }
    int add = -1;
    if (w >= input[k].weight) {
        add = _recursive(input, k - 1,
                         w - input[k].weight,
                         memo, mark) + input[k].value;
    }
    int not_add = _recursive(input, k - 1, w, memo, mark);
    if (add > not_add) {
        memo[k][w] = add;
        mark[k][w] = 1;
    } else {
        memo[k][w] = not_add;
        mark[k][w] = 0;
    }
    return memo[k][w];
}

int max_value_memoized(std::vector<Knapsack>& input, std::vector<Knapsack>& result,
                       int max_weight) {
    std::vector<std::map<int, int> > memo;
    std::vector<std::map<int, int> > mark;
    memo.reserve(input.size());
    mark.reserve(input.size());
    for (uint i = 0; i < input.size(); ++i) {
        memo[i] = std::map<int, int>();
        mark[i] = std::map<int, int>();
    }
    _recursive(input, input.size() - 1, max_weight, memo, mark);
    int w = max_weight;
    for (int i = input.size() - 1; i >= 0; --i) {
        if (mark[i][w] == 1) {
            result.push_back(input[i]);
            w -= input[i].weight;
        }
    }
    return memo[input.size() - 1][max_weight];
}

int max_value(std::vector<Knapsack>& input, std::vector<Knapsack>& result,
              int max_weight, bool bottom_up) {
    if (bottom_up) {
        return max_value_bottom_up(input, result, max_weight);
    } else {
        return max_value_memoized(input, result, max_weight);
    }
}
```
