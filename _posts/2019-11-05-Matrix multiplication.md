---
layout: post
title: "Matrix Multiplication"
description: "직관적 이해"
comments: true
categories: [Misc]
tags:
- Matrix multiplication
---

From [Learn Again!](https://twlab.tistory.com/10) and [11-741](http://nyc.lti.cs.cmu.edu/classes/11-741/f19/index.html).

## Matrix-vector multiplication

### Weighted sum of columns

- $Ax$ is linear combination of columns of $A$.
- The weights are given by the elements of $x$.

### Weighted sum of rows

* $x^TA$ is linear combination of rows of $A$.
* The weights are given by the elements of $x^T$.



## Matrix-diagonal matrix multiplication

### Weights on columns

* A column in $A\Lambda$ = a column of $A$ multiplied by the weight.
* The weights are given by the diagonal elements.

### Weights on rows

- A row in $\Lambda B$ = a row of $B$ multiplied by the weight.
- The weights are given by the diagonal elements.



## Matrix-matrix multiplication

$$
AB = C
$$

### Row * column

$$
C_{i,j} = \sum \limits_{k} A_{i,k}B_{k,i}
$$

- A element in C = inner product of a row in A and a column in B.

### Column * row

$$
C = \sum\limits_{i} a_ib_i^T
$$

- C is a sum of outer product of columns in A and rows in B.

### Column-wise

- A column in C = a weighted combination of the columns of A. 
- The weights are given by a columns in B.

### Row-wise

- A row in C = a weighted combination of the rows in B
- The weights are given by a row in A.



## Useful identities

$$
VV^T = \sum \limits_i^n v_iv_i^T \\
V \Lambda V^T = \sum_i^n \lambda_i v_i v_i^T \\
$$

$$
Z^TLZ =
$$

- Notation for a vector and a scalar can be confusing here.

$$
(V^TV)_{i,j} = v_i^Tv_j \\
$$

$$
u^T \Lambda u = \sum \limits_i \lambda_i u_iu_i\\
u^TAu = \sum_i \sum_j u_i u_j A_{i,j} \\
$$



