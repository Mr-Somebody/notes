---
title: Latent Dirichlet Allocation
category: Machine Learning
---
## Related Conceptions
### Beta Distribution
### Gamma Distribution
### Dirichlet Distribution
### k-dimension Dirichlet Distribution
### LSI/LSA (Latent Semantic Indexing/Analysis)
### PLSI/PLSA (Probabilistic LSI/LSA)
### Exchangeability

## Usages
1. Topic Model
2. Collaborative Filtering
3. Content-basd Image Retrieval
4. Bioinformatics

## Assumptions
1. Choose \\(N \\sim Poisson(\\xi)\\)
2. Choose \\( \\theta \\sim Dir(\\alpha)\\)
3. For each of the N words \\(w_{n}\\):
    - Choose a topic \\(z_{n} \\sim Multinomial(\\theta)\\).
    - Choose a word \\(w_{n}\\) from \\(p(w_{n}\|z_{n}, \\beta) \\), a Multinomial probability conditioned on the topic \\(z_{n}\\).

## Graphic Model Representation
![Graphic Model Representation of LDA](graphic model representation of LDA.png "Graphic Model Representation of LDA")
- Three levels of LDA representation
    - \\(\\alpha\\) and \\(\\beta\\) are corpus parameters, assumed to be sampled once in the process of generating a corpus.
    - \\(\\theta\_{d}\\) are document-level variables, sampled once per document.
    - \\(z_{dn}\\) and \\(w_{dn}\\) are word-level variables and are sampled once for each word in each document.

## Model
Given the parameter \\(\\alpha\\) and \\(\\beta\\), the joint distribution of a topic mixture \\(\\theta\\), a set of N topics z, and a set of N words w is given by:

\\[p(\\theta,z,w\|\\alpha,\\beta)\\]

Integrating over \\(\\theta\\) and summing over \\(z\\), we obtain the marginal distribution of a document:

\\[\\]

Taking the product of the marginal probability of single document, we obtain the probability of a corpus:

\\[\\]

## Relation with other latent variable models

## Posterior
Posterior distribution of the hidden variables given a document(Intractable):

\\[\\]

Normalize the distribution over hidden variables:

\\[\\]

## References
1. Blei, David M.; Ng, Andrew Y.; Jordan, Michael I (January 2003). Lafferty, John, ed. "Latent Dirichlet Allocation". Journal of Machine Learning Research. 3 (4–5): pp. 993–1022. doi:10.1162/jmlr.2003.3.4-5.993
