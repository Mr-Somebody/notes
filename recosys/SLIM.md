# SLIM(Sparse LInear Method)

## Introduction
SLIM is method to solve top-N recommendation problem. It formalize the problem as a optimze problem.

## Model
Suppose A is the rating matrix, in which the (i, j) entry (denoted by \\(a_{ij}\\)) is 1 if user i has ever rated item j, otherwise, 0. Let \\(\hat{a}_{ij}\\) denotes the estimated recommendation score on a un-rated item \\(t_{j}\\) of user \\(u_{i}\\). We calculate \\(\\hat{a}_{ij}\\) use formula below:

\\[\\hat{a}_{ij}=a^{T}_{i}w_{j}\\]

where \\(w_{j}\\) is a sparse column vector of *aggregation coefficients*. Thus, the model utilized by SLTM can be resprensed as

\\[\\hat{A}=AW\\]

where A is estimated recommendation score matrix, and the size of which is \\(m \\times n\\) (m is number of users and n is number of items). W is a \\(n \\times n\\) matrix.

To get \\(\\hat{A}\\), we need to calculate the matrix \\(W\\), which is the optimize variable of the model.

## optimzztion function
The optimization function of SLIM is formalized as below:

\\[\\mathop{minimize}\\limits_{W}\\quad\\frac{1}{2}||A-AW||_{F}^{2}+\\frac{\\beta}{2}||W||_{F}^{2}+\\lambda||W||_{1}\\]

\\[subject\\quad to\\quad W\\]
