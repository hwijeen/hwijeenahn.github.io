---
layout: post
title: "Quadratic Functions"
description: "수식으로 주어진 함수의 모양 파악하기"
comments: true
categories: [Mathematics]
tags:
- Deep Learning
- Quadratic Function
---



> 딥러닝을 비롯한 여러 머신러닝 알고리즘은 목적함수를 최소화하는 문제로 귀결된다. 그 과정에서 quadratic funtion이 자주 등장한다. 설령 목적함수가 quadratic funtion이 아니더라도, taylor expansion을 통해 특정 점 주위에서 이차 근사를 시킬 수 있다. Quadratic function이 주어졌을 때, 그 함수의 모양을 어떻게 파악하는 지에 대한 내용을 정리해둔다.  [Neural Network Design](http://hagan.okstate.edu/NNDesign.pdf) 8장, [다크프로그래머님의 블로그](http://darkpgmr.tistory.com/132) 을 참고했다. 



### Quadratic function

$$
\mathbf{F(x) = \frac{1}{2} x^\intercal Ax + d^\intercal x + c}
$$

일반적인 형태는 위와 같이 쓸 수 있다. 이때 $\mathbf{x}$는 벡터임에 유의한다. 여기서 다루는 함수는 변수가 $x_1, x_2, \dots, x_n$와 같이 여러 개인 multivariate functions이다. 그래프로 생각을 하려면 x축과 y축 위에 하나의 선으로 표현되는 함수가 아니라, 3차원 공간에 입체적으로 존재하는 surface 떠올려야한다.

일차 미분은 기울기를, 이차 미분은 기울기의 변화량을 나타낸다고 배웠다. 이러한 성질을 이용해서 함수의 모양을 파악할 거니까 재료를 준비해둔다. [**함수의 Gradient**는 각 변수에 대한 일차 편미분 값을 지닌 벡터를, **Hessian**은 함수의 이차미분을 담고있는 행렬이다.](http://darkpgmr.tistory.com/132) 
$$
\mathbf{\nabla F(x) = Ax + d}  \\
\mathbf{\nabla^2 F(x) = A}
$$


### 함수의 모양

식으로 주어진 quadratic function의 모양을 알아내는 과정을 말로 하면 다음과 같다. 이차 함수기에 모양이란 건 위로 볼록, 아래로 볼록 혹은 saddle point가 있을 수 있다. 즉, maximum을 갖거나, minimum을 갖거나, saddle point를 갖거나 세 경우 중 어느 것에 속하는 지를 알아내야한다. **알아내는 방법은 기울기의 변화량, 즉 이차 미분값을 보는 것!** 모든 기저 방향으로 기울기가 증가한다면 이차 함수는 아래로 볼록하다. 모두 감소한다면 위로 볼록한 모양, 어떤 방향으로는 감소 다른 방향으로는 증가한다면 saddle point를 가질 거라고 생각할 수 있다.

**이때 공간의 기저를 특정 벡터로 잡으면, 그 방향으로의 기울기를 쉽게 구할 수 있다.** 그러니까 우리가 해야할 일은 1) 기저 변환 2) 변환한 기저방향들로의 이차 미분값 구하기 두 가지다.

### Eigensystem of the Hessian

지금까지는 함수 $\mathbf{F(x)}$가 존재하는 공간 $\mathbb{R^n}$을 나타낼 때 기저 벡터로 $x_1, x_2, \dots, x_n$(표준 기저)를 이용했다. 물론 이 기저 벡터들 방향으로의 이차 미분값을 다 구해보면 이차 함수의 모양을 알 수 있다. 그런데, 기저 벡터를 Hessian의 eigenvector로 바꿔서 함수를 표현하면, **기저 방향으로의 이차 미분값이 eigenvalue로 쉽게 정리되는 상황이 된다!** 

> ㅈ어떤 공간의 기저 벡터는 서로 linearly independent해야한다. 
>
> Hessian $\mathbf{A}$는 symmetric matrix이므로 eigenvector들이 서로 orthogonal하다. 서로 orthogonal한 벡터들은 linearly independent하다(충분조건). 그러므로 Hessian의 eigenvector들을 공간의 기저 벡터로 사용할 수 있다. 



재료준비
$$
\mathbf{B = [v_1, v_2, \dots, v_n]} \\
\text{then} \quad \mathbf{B^{-1} = B^\intercal}
$$
행렬 $\mathbf{B}$는 기저 벡터를 열벡터로 하는 벡터다.$\mathbf{B^\intercal B}$를 계산해보면 직교하는 두 벡터의 내적은 0이라는 성질 때문에 $\mathbf{B^\intercal B = I}$, 즉 $\mathbf{B^{-1} = B^\intercal}$를 얻어낼 수 있다. 이 성질을 이용해 행렬 $\mathbf{A}$를 eigendecomposition하면 다음과 같다. A

> nxn 행렬이 n개의 linearly independent한 eigenvector를 가질 때 eigendecomposition을 할 수 있다.

$$
\mathbf{A = B \Lambda B^-1 = B \Lambda B^\intercal}
$$

한편, 임의의 방향 