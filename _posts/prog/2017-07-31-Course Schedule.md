---
title: Course Schedule
category: Programming
comments: true
excerpt_separator: <!--more-->
date: 2017-07-31 11:11:00
---
here are a total of n courses you have to take, labeled from 0 to n - 1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?

For example:

>2, [[1,0]]

There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

>2, [[1,0],[0,1]]

There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/course-schedule)

## Methodology
This is a classical problem, which will be included in every DS or Algorithm textbook. The keynote is to detect if this graph exits a circle. If does, we can not finish all courses; Otherwise we can. In the textbook, we call a directed graph without circles DAG (Directed Acyclical Graph). To determin if a graph is DAG, we recursively remove the nodes whose indegree is zero (In this problem is the course without prerequests) util no more nodes can be removed. If all nodes are removed, it means the graph is a DAG (which also means we can finish all courses); Otherwise not.

For a DAG, we need to traverse all notes and edges (When remove a nodes, we need to decrease indegrees of all nodes this node points to). So the total time complexity is O(n + e), in which n is the number of nodes and e is the number os edges.

## Source Code
```C++
class Solution {
public:
    bool canFinish(int numCourses, std::vector<std::pair<int, int> >& req) {
        //
        std::map<int, int> counter;
        for (uint i = 0; i < numCourses; ++i) {
            counter[i] = 0;
        }
        //
        std::map<int, std::vector<int> > rmap;
        for (uint i = 0; i < req.size(); ++i) {
            if (rmap.find(req[i].second) == rmap.end()) {
                rmap[req[i].second] = std::vector<int>();
            }
            rmap[req[i].second].push_back(req[i].first);
            ++counter[req[i].first];
        }
        //
        std::queue<int> q;
        for (uint i = 0; i < numCourses; ++i) {
            if (counter[i] == 0) {
                q.push(i);
            }
        }
        //
        while (q.size() > 0) {
            int zero = q.front();
            printf("zero=%d\n", zero);
            q.pop();
            counter.erase(zero);
            if (rmap.find(zero) != rmap.end()) {
                for (uint i = 0; i < rmap[zero].size(); ++i) {
                    --counter[rmap[zero][i]];
                    if (counter[rmap[zero][i]] == 0) {
                        q.push(rmap[zero][i]);
                    }
                }
            }
        }
        return counter.size() == 0;
    }
};
```
