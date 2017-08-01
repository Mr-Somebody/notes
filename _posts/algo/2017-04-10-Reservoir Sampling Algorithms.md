---
title: Reservoir Sampling Algorithms (RSA)
category: Algorithms
comments: true
excerpt_separator: <!--more-->
layout: post
tags: [sampling, randomize, list]
---
Reservoir sampling is a family of randomized algorithms for randomly choosing a sample of k items from a list S containing n items, where n is either a very large or unknown number. Typically n is large enough that the list doesn't fit into main memory.
<!--more-->

## Methodology
For the code is simple, here only describes the proof of uniformity of this algorithm.
Suppose we have selected *k* items from *i* items uniformly, which means that every item is selected by probability of *k/i*. For the *(i+1)th* item, we need to decide whether to pick or not. If we pick the *(i+1)th* item, we have to drop one from the already picked *k* item.
We make the probability of pick *(i+1)th* item to be *k/(i+1)*, then the probability that a item already picked not to be dropped is *1-k/(i+1) + k/(i+1) * (k-1)/k*, which is *i/(i+1)*. So for a item already in reservoir, the total probability in picked is *k/i * i/(i+1)*, which is also *k/(i+1)*.

## References
[Wikipedia](https://en.wikipedia.org/wiki/Reservoir_sampling)

## Further Reading
[Map-Reduce Reservoir Sampling](http://had00b.blogspot.hk/2013/07/random-subset-in-mapreduce.html)

## Source Code
```C++
#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <algorithm>

void reservoir_sampling(std::vector<int> &source, std::vector<int> &des, uint k) {
    des.clear();
    uint nk = std::min(k, (uint)source.size());
    uint i = 0;
    for (; i < nk; ++i) {
        des.push_back(source[i]);
    }
    while (i < source.size()) {
        int j = rand() % i;
        if (j <= nk) {
            des[j] = source[i];
        }
        ++i;
    }
}
```
