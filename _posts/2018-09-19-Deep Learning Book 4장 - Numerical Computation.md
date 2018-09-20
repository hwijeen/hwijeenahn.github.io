---
layout: post
title: "Deep Learning Book 4장 - Numerical Computation"
description: "딥러닝에 필요한 기초 수학 및 기타 이론"
comments: true
categories: [NLP]
tags:
- Deep Learning
- Numerical Computation
---



> 4장은 Numerical Computation을 다룬다. 어떤 문제를 풀고자 할 때, 즉 어떤 값을 찾고자할 때 그 방법은 크게 두 가지가 있다. Analytic한 방법과 Numeric한 방법. 후자는 정답 추정치를 반복적으로 update하면서 답을 구한다. 이와 관련된 내용 정리. [Neural Network Design](http://hagan.okstate.edu/NNDesign.pdf)이라는 책도 참고했다. 



## Numerical Computation

1. Overflow and Underflow

무한히 많은 실수를 유한한 메모리의 컴퓨터가 다루다보니 approximation error가 발생할 수밖에 없다. '반올림 오류rounding error'라고 하면 사소해보일 수 있으나 복잡한 연산 과정에서 이 오류가 계속되면 결과는 치명적이다. Underflow는 0에 가까운 아주 작은 값을 0으로 반올림해버리는 오류다. 분모가 0이 되어버리면 값 계산 자체가 불가능해져버린다. Overflow는 너무 큰 값을 $\infty$로 처리해버리는 경우이다. Softmax 함수 구현 코드를 보면 모든 값에서 최대치를  빼버리는 과정이 있는 게 이런 이유에서다. 너무나 많은 이슈가 있기 때문에 좋은 라이브러리를 믿고 쓰는 게 현명한 선택이다. 

2.  Poor Conditioning

Conditioning은 input이 조금 변할 때 함수값이 얼마나 빠르게 변하는지를 알려주는 척도이다. Poor conditioned된 함수의 경우, input의 반올림 오류가 output에서는 엄청나게 커질 수도 있다. Condition number은 아래와 같이 matrix의 eigenvalue를 사용해서 계산 되며, **matrix가 갖는 특성**임에 유의하자.
$$
f(\mathbf{x}) = \mathbf{A}^{-1}\mathbf{x} \\
\max_{i,j} \left|\frac{\lambda_i}{\lambda_j} \right|
$$
(p.89) The condition number of Hessian measures how much the second derivatives vary.

3. Gradient-Based Optimization

최적화는 $\mathbf{x}$를 조정해서 $f(\mathbf{x})$의 최소화, 최대화 하는 문제를 일컫는다. Objective function, cost function, error function 등은 모두 최소화 또는 최대화하고자 하는 함수 $f(\mathbf{x})$를 말한다. 

$f(\mathbf{x})$의 도함수는 어떤 지점에서 $f(\mathbf{x})$의 기울기 정보를 담고 있다. 만약 input을 1개만 가지는 univariate function의 경우 $f'(\mathbf{x})$으로 표기하며, 그 값이 0인 지점을 critical point라고 한다. Critical point는 minimum일 수도, maximum일 수도, saddle point일 수도 있다. 여러 개의 input을 가지는 multivariate function의 경우엔 **편미분** 개념을 도입한다. $\frac{\partial}{\partial{x_i}}f(\mathbf{x})$는 $\mathbf{x}$ 지점에서 변수 $x_i$가 증가함에 따라 함수 $f$가 얼마나 변하는지를 나타낸다. 모든 변수에 대한 양상을 다 모아서 **gradient** vector $\nabla_{x}f(\mathbf{x})$로 표현한다. 특정 방향 $\mathbf{u}$에 대한 **directional derivative**는 

4. Jacobian and Hessian Matrices

Hessian을 이용해서 critical point가 minimum인지, maximum인지 확인하기

~ chapter 8 of Neural Netwrok Design, necessary conditions for optimality