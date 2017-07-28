---
title: Number of Islands
category: Programming
comments: true
excerpt_separator: <!--more-->
---
## Description
>Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
<!--more-->

[Leetcode Link](https://leetcode.com/problems/number-of-islands/#/description)

## Methodology
This is typical graph traversal problem. We can treat this 2d grid as a graph, and traverse it from the left-top corner to the right-bottom corner. During the traversal, once we meet a '1', we treat that position as a start point a the graph, an traverse the graph using dfs or bfs. During dfs or bfs or traverse, we only traverse the position whose value is '1' recursively. To avoid repetive traverse of '1', we need to change '1' to 'x' when we meet '1'. After the traverse operation, the number of '1's we met is the num of islands.

## Source Code
```C++
class Solution {
public:

    void dfs(vector<vector<char> >& grid, uint i, uint j) {
        grid[i][j] = 'x';
        if (i > 0 && grid[i - 1][j] == '1')
            dfs(grid, i - 1, j);
        if (i < grid.size() - 1 && grid[i + 1][j] == '1')
            dfs(grid, i + 1, j);
        if (j > 0 && grid[i][j - 1] == '1')
            dfs(grid, i, j - 1);
        if (j < grid[i].size() - 1 && grid[i][j + 1] == '1')
            dfs(grid, i, j + 1);
    }

    int numIslands(vector<vector<char> >& grid) {
        int mark = 2;
        for (uint i = 0; i < grid.size(); ++i) {
            for (uint j = 0; j < grid[i].size(); ++j) {
                if (grid[i][j] == '1') {
                    dfs(grid, i, j);
                    ++mark;
                }
            }
        }
        return mark - 2;
    }
};
```
