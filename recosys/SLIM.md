# SLIM(Sparse LInear Method)

## Introduction
SLIM is method to solve top-N recommendation problem. It formalize the problem as a optimze problem.

## Model
Suppose A is the rating matrix, in which the (i, j) entry (denoted by \\(a_{ij}\\)) is 1 if user i has ever rated item j, otherwise, 0. Let \\(\\hat{a}\_{ij}\\) denotes the estimated recommendation score on a un-rated item \\(t\_{j}\\) of user \\(u_{i}\\). We calculate \\(\\hat{a}_{ij}\\) use formula below:

\\[\\hat{a}\_{ij}=a^{T}\_{i}w_{j}\\]

where \\(w_{j}\\) is a sparse column vector of *aggregation coefficients*. Thus, the model utilized by SLTM can be resprensed as

\\[\\hat{A}=AW\\]

where A is estimated recommendation score matrix, and the size of which is \\(m \\times n\\) (m is number of users and n is number of items). W is a \\(n \\times n\\) matrix.

To get \\(\\hat{A}\\), we need to calculate the matrix \\(W\\), which is the optimize variable of the model.

## optimzation function
The optimization function of SLIM is formalized as below:

\\[\\mathop{minimize}\\limits\_{W}\\quad\\frac{1}{2}\|\|A-AW\|\|\_{F}^{2}+\\frac{\\beta}{2}\|\|W\|\|\_{F}^{2}+\\lambda\|\|W\|\|\_{1}\\]

\\[subject\\quad to\\quad W \\ge 0, \\quad\\quad diag(W)=0\\]

where \\( \|\|W\|\|\_{1}=\\sum^{n}\_{i=1}\\sum^{n}\_{j=1}\|w\_{ij}\|\\) is entry-wise \\(l\_{1}-norm\\) of \\(W\\), which is used to introduce sparsity into the solutions. \\(\\frac{\\beta}{2}\|\|W\|\|\_{F}^{2}\\) is the \\(l\_{F}-norm\\) of \\(W\\), which is used to reduce the complexity of the model and prevents overfitting.

## Optimization trick
Since the columns of \\(W\\) are independent, we can calculate each column of \\(W\\) parellel. The optimization problem can be transform to the follow form:

\\[
\\mathop{minimize}\\limits\_{w\_{j}}\\quad\\frac{1}{2}\|\|a\_{j}-Aw\_{j}\|\|\_{2}^{2}+\\frac{\\beta}{2}\|\|w\_{j}\|\|\_{2}^{2}+\\lambda\|\|w\_{j}\|\|\_{1}
\\eqno{(1)}
\\]

\\[subject\\quad to\\quad w\_{j} \\ge 0, \\quad\\quad diag(w\_{i,j})=0\\]

where \\(w\_{j}\\) is the j-th colum of \\(W\\).

## Ranking TOP-N items by SLIM

The sparsity of \\(W\\) enables significantly faster top-N recommendation.
