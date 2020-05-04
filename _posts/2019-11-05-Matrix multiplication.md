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

###Weighted sum of columns

![image-20191105155704012](/Users/hwii/Library/Application Support/typora-user-images/image-20191105155704012.png)

- $Ax$ is linear combination of columns of $A$.
- The weights are given by the elements of $x$.

### Weighted sum of rows

![image-20191105153615011](file:///Users/hwii/Library/Application%20Support/typora-user-images/image-20191105153615011.png?lastModify=1572986243)

* $x^TA$ is linear combination of rows of $A$.
* The weights are given by the elements of $x^T$.



## Matrix-diagonal matrix multiplication

### Weights on columns

![image-20191105155257809](/Users/hwii/Library/Application Support/typora-user-images/image-20191105155257809.png)

* A column in $A\Lambda$ = a column of $A$ multiplied by the weight.
* The weights are given by the diagonal elements.

### Weights on rows

![image-20191105155310859](/Users/hwii/Library/Application Support/typora-user-images/image-20191105155310859.png)

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

###Column-wise

![image-20191105154505379](/Users/hwii/Library/Application Support/typora-user-images/image-20191105154505379.png)

- A column in C = a weighted combination of the columns of A. 
- The weights are given by a columns in B.

###Row-wise

![image-20191105154442420](/Users/hwii/Library/Application Support/typora-user-images/image-20191105154442420.png)

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

![image-20191118221843468](/Users/hwii/Library/Application Support/typora-user-images/image-20191118221843468.png)

- Notation for a vector and a scalar can be confusing here.

$$
(V^TV)_{i,j} = v_i^Tv_j \\
$$

$$
u^T \Lambda u = \sum \limits_i \lambda_i u_iu_i\\
u^TAu = \sum_i \sum_j u_i u_j A_{i,j} \\
$$



