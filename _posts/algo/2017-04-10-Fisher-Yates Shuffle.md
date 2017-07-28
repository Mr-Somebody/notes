---
title: Fisher-Yates Shuffle
category: Algorithms
comments: true
excerpt_separator: <!--more-->
---
The Fisher–Yates shuffle is an algorithm for generating a random permutation of a finite set—in plain terms, the algorithm shuffles the set. The algorithm effectively puts all the elements into a hat; it continually determines the next element by randomly drawing an element from the hat until no elements remain. The algorithm produces an unbiased permutation: every permutation is equally likely. The modern version of the algorithm is efficient: it takes time proportional to the number of items being shuffled and shuffles them in place.
<!--more-->

The Fisher–Yates shuffle is named after Ronald Fisher and Frank Yates, who first described it, and is also known as the Knuth shuffle after Donald Knuth. A variant of the Fisher–Yates shuffle, known as Sattolo's algorithm, may be used to generate random cyclic permutations of length n instead of random permutations.

## Methodology

## References
[Wikipedia](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)

## Source code
```C++
// The "inside-out" algorithm: implemented by Durstenfeld
void shuffle(std::vector<int> &source, std::vector<int> &des) {
    des.clear();
    for (int i = 0; i < (int)source.size(); ++i) {
        int j = rand() % (i + 1);
        if (j != i) {
            des.push_back(des[j]);
        } else {
            des.push_back(-1);
        }
        des[j] = source[i];
    }
}

// The modern algorithm: introduced by Richard Durstenfeld in 1964[2] and popularized by Donald E. Knuth in The Art of Computer Programming as "Algorithm P"
void shuffle(std::vector<int> &source) {
    for (int i = (int)source.size(); i > 0; --i) {
        int k = rand() % i;
        int tmp = source[i - 1];
        source[i - 1] = source[k];
        source[k] = tmp;
    }
}
```
