---
layout: post
title: "Multivariate Functions"
description: "Multivariate Functions 기본 지식 및 표현 방법" 
comments: true
categories: [Math & Stats]
tags:
- Multivariate Calculus
---



[Khan Academy](https://www.khanacademy.org/math/multivariable-calculus), [다크 프로그래머의 블로그](http://darkpgmr.tistory.com/132), [cs224n lecture note](https://web.stanford.edu/class/cs224n/readings/gradient-notes.pdf)

<br>

### 함수의 종류: input & ouput space 차원에 따라

$$
\begin{align}
\mathbb{R} &\rightarrow \mathbb{R} \\
\mathbb{R}^n &\rightarrow \mathbb{R} \\
\mathbb{R}^n &\rightarrow \mathbb{R}^n \\
\mathbb{R} &\rightarrow \mathbb{R}^n
\end{align}
$$

output value가 $\mathbb{R}$인 함수를 **single-valued function**이라고 부르고, output value가 $\mathbb{R}^n$인 함수를 vector-valued function이라고 부른다.

multivariate function이라 함은 보통 input이 $ \mathbb{R}^n $인 함수를 일컫는다.

<br>

### Visualizing functions: seeing connection between input & output space

- Graph: input & output space를 한 공간에서 표현한다
  - $ \mathbb{R}\rightarrow\mathbb{R} $ , $\mathbb{R}^2 \rightarrow \mathbb{R}$밖에 표현하지 못함($\mathbb{R}^2 \rightarrow \mathbb{R}$는 잘 안 씀)
  - $\mathbb{R}^2 \rightarrow \mathbb{R}$인 경우, (x,y,f(x,y)) 를 하나의 점으로, 3차원 공간에 표현
- Contour maps: input space를 표현하고, 그 값이 mapping 되는 output 지점을 표현한다.
  - $\mathbb{R}^2 \rightarrow \mathbb{R}$을 표현하는 그래프이다. 
  - 등고선은 output 기준으로 evenly spaced. 
- Parametric functions: output space를 표현
- Vector field: ~~모르기로 한다~~
- Transformation: 하나의 공간에서 다른 공간으로 transform!
  - 차원에 국한되지 않는 표현이 가능하다. 

<br>

#### TODO:

Derivates of multivariable functions

















