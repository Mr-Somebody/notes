---
title: Double Array Trie
category: Structure
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2017-08-23 11:30:00
tags: [array, trie, tree, python]
---
Double Array Trie, short for DART, is a representation of Trie Tree. It's well known for its fast search operation. If one need to construct a trie tree which is search intensive, he/she can consider DART as his/her best choice.
<!--more-->

## Methodology
Intuitively, DART is consist of two arrays, a base array and a check array. The base array is used for search and the check array is used to determine is the search position come from the preposition. Let's introduce the construction of these two array by an example.

Suppose we need to construct a trie for two words, say "come" and "go". Following is some assumptions:
1. Let B represents the base array and C represents the check array.
2. Let B[i] and C[i] represents the values at B and C respectively.
3. Let int(c) represents the integer value of character c.
4. Let k represents the minimum unused base array position.

Here is the method construct a DART:
1. Construct a trie using ordinary tree structure.
2. Converted this trie in to DART representation.

There's a way to construct DART directively, which I think is kind of complicated. So here we use a extra step to make it more clear.

Considering a trie tree as follows:

It consist of two words: "come" and "go". To convert thie trie tree into  DART representation, we do following steps:
1. Set B[0]=0 as the root of the trie.
2. Set C[0+int(c)]=B[0], which means character 'c' comes from the root. Then search base position for character 'c', say T, and set B[0+int(c)]=T.
3. Set C[T+int(o)]=T, which means character 'o' comes from character 'c' at position 0+int(c). Then search base position for character 'o'.
4. For the remaining characters 'm' and 'e', do the same process as character 'o'. However, the last character indicates a full word. To represent this, Set C[B[e]]=B[e].

In inserting a character, there's a need to search for the base position. To do this, we first try the current unused base position k, and check if all the position for its children nodes in C are unused. If true, make k as the base position. Otherwise, we try increase k util we find an available postion.

After we construct a DART, the search process is easy. Also take "come" for example. First, set root as the current node. The we first goto position B[0]+int(c) and check if C[B[0]+int(c)] equals B[0], if true, it means c comes from root. Then we set c as current node. Repete this operation util finished searching or we meet a position check array value not equals. The former means we find the word. Otherwise not. Besides, after we finish the whole word search, we need to check if and only if check value of the last word equals to its base value. If true, it means it forms a complete word. For example, we can find "com" in the DART, but it's not a word.

## Source Code
```Python
class TrieTreeNode(object):
    """Trie tree node
    """
    def __init__(self, val="", is_term=False):
        """
        Args:
        val: value of the node
        is_term: is the node a term
        """
        self.val = val
        self.is_term = is_term
        self.children = {}

class TrieTree(object):
    """Trie tree class
    """
    def __init__(self):
        """
        """
        self.root = TrieTreeNode()

    def to_dart(self):
        """Convert trie tree to double array. Request the node key to be int

        Return:
        (base, check)
        """
        def _resize():
            self.base = self.base + [0] * 65535 * 2
            self.check = self.check + [0] * 65535 * 2
            self.data = self.data + [0] * 65535 * 2
        def _find_base(rnode):
            pos = self.min_unused_base
            i = 0
            children = rnode.children.keys()
            if rnode.is_term:
                children = [0] + children
            children.sort()
            while i < len(children):
                while len(self.check) < children[i] + pos:
                    _resize()
                if self.check[children[i] + pos] != 0:
                    i = 0
                    pos = children[i] + pos + 1 - children[0]
                i += 1
            return pos
        def _recurse(rnode, bpos):
            if rnode.is_term:
                self.check[self.base[bpos]] = self.base[bpos]
                self.data[self.base[bpos]] = rnode.val
            for v in rnode.children:
                self.check[self.base[bpos] + v] = self.base[bpos]
                cbase = _find_base(rnode.children[v])
                self.base[self.base[bpos] + v] = cbase
                self.min_unused_base = cbase + 1
                _recurse(rnode.children[v], self.base[bpos] + v)
        self.base = []
        self.check = []
        self.data = []
        self.min_unused_base = 2
        _resize()
        self.base[0] = 1
        self.check[0] = 1
        _recurse(self.root, 0)
        return (self.base, self.check, self.data)

    def match_all_dart(self, vlist):
        """Find all matches of double array representation of trie tree

        Args:
        vlist: value list

        Return:
        All match lens
        """
        lens = []
        b = 0
        for i in xrange(0, len(vlist)):
            v = vlist[i]
            if self.base[b] + v < len(self.check):
                if self.check[self.base[b] + v] == self.base[b]:
                    b = self.base[b] + v
                    if self.check[self.base[b]] == self.base[b]:
                        lens.append(i + 1)
        return lens
```
