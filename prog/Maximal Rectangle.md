---
title: Maximal Rectangle
category: Programming
comments: true
---
## Description
>Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

[Leetcode Link](https://leetcode.com/problems/maximal-rectangle/#/description)

## Methodology
At first glance, the brute force method is to traverse all possible rectangles in the matrix to get the maximal area one. For each rectange, we can define it by its two corner point, say (a, b) and (c, d). The total rectange can be O((nm)^2), which is very big while m and n grow bigger.

The brute force method is definately not a good method for solving this problem. Rescan the problem description, we can find that this problem is a optimal solution problem, which means we may solve it using DP (Dynamic programming). So let's try this.
1. Methodology: Traverse the matrix from the top-left to bottom-right, and calculate maximal rectangle within current row and column.
2. Symbols Defination:
    1. Let F(i, j) denotes the maximal rectangle area within the row i and column j (which means the rectangles must be in the submatrix (0, 0)=>(i,j))
    2. Let G(i, j) denotes the maximal height of the point (i, j), which means the number of 1s from (i, j) up-forward. For example, if A[i][j]=1, A[i-1][j]=1 and A[i-2][j]=2, then G(i, j)=2; If A[i-1][j]=0, then G(i, j) = 0;
    3. Let Z(i, j) denotes the maximal rectangle area contains point (i, j).
3. Optimal Subproblems: With the definations above. The problems we need to solve for a matrix of n lines and m columns is to calculate F(n, m). We can also define the derivation as follows:

\\[ G(i, j) = 0 \\quad\\quad if \\quad\\quad matrix[i - 1][j] == 0; \\quad Otherwise, \\quad\\quad G(i, j) = G(i - 1, j) + 1 \\]
\\[ F(i, j) = max(Z(i, j), F(i - 1, j), F(i, j - 1)) \\]

4. Overlapping: When we calculate G(i, j), we need to calculate G(i - 1, j). When we calcualte G(i - 1, j), we need to calculate G(i - 2, j). We calculate G(i - 2, j) twice when calculate G(i, j) and G(i - 1, j).

5. Time Complexity: For we need to calculate F(i, j) for each point, so we need to calculate at least nm calculations. However, when we calculate Z(i, j), We need traverse line i backward from point (i, j), util meet a point with value 0. So the overall time complexity of this method is O(nm^2). When m is much larger than n, we can reverse row and column to get better efficiency.

6. Space complexity: From the derivation fomulas, we can see that we do not need to store every F(i, j) and G(i, j), we only need to store the value before current line. So the space complexity is O(m).

7. Border Conditions:
    1. First line: For the first line, G(0, j) = 0. F(0, j) is longest consecutive 1s backward.
    2. First column: For the first column F(i, j - 1) can be treated as zero.

## Source Code
```C++
class Solution {
public:
    int maximalRectangle(std::vector<std::vector<char> >& matrix) {
        if (matrix.size() < 1 || matrix[0].size() < 1) return 0;
        std::vector<int> F(matrix[0].size(), 0);
        std::vector<int> G(matrix[0].size(), 0);
        int len = 0;
        for (int i = 0; i < F.size(); ++i) {
            if (matrix[0][i] == '1') ++len;
            else len = 0;
            F[i] = std::max(F[i - 1], len);
        }
        for (int row = 1; row < matrix.size(); ++row) {
            for (int col = 0; col < matrix[0].size(); ++col) {
                int maxv = 0;
                if (matrix[row][col] == '0') {
                    G[col] = 0;
                } else {
                    G[col] = matrix[row - 1][col] == '0' ? 0 : 1 + G[col];
                    int k = col;
                    int height = G[col];
                    while (k >= 0 && matrix[row][k] == '1') {
                        //printf("row=%d, col=%d, k=%d, height=%d\n", row, col, k, height);
                        maxv = std::max(maxv, (col - k + 1) * (height + 1));
                        --k;
                        height = std::min(height, G[k]);
                    }
                }
                F[col] = std::max(std::max(col > 0 ? F[col - 1] : 0, maxv), F[col]);
            }
        }
        return F[matrix[0].size() - 1];
    }
};
```
