---
layout: post
title: "Deep Learning Book 2 - Linear Algerbra"
description: "딥러닝에 필요한 기초 수학 및 기타 이론"
comments: true
categories: [NLP]
tags:
- Deep Learning
- Linear Algebra
---



> Deep Learning Book 2,3,4장은 딥러닝에 필요한 기본적 지식을 다룬다(1장은 역사 등등). 2장 Linear Algebra에서 확실히 알아두어야할 정의 및 개념, 수식의 의미 등을 위주로 다시 정리해둔다.



## Linear Algebra

우리가 풀려는 문제를 써놓으면 $ \mathbf{A}\mathbf{x} = \mathbf{b}$꼴이 되는 경우가 많다. 이론적으로 볼 땐, 양변에 $\mathbf{A}^{-1}$을 곱해줌으로써 $\mathbf{x}$값을 closed-form으로 구할 수 있다.그러나 $\mathbf{A}^{-1}$을 컴퓨터로 계산하는 과정에서 precision 이슈가 생기기 때문에 closed form 해법은 실전에서 사용하지 않는다. 위의 식은 $\mathbf{b}$의 값에 따라 위의 식은 해가 없거나, 하나의 해를 가지거나, 무수히 많은 해를 가진다. 셋 중 어느 경우인지를 따져보려면 다음과 같이 생각해보면 된다. $ \mathbf{A}$의 column vector들을 조합해서  $\mathbf{b}$에 도달하는 방법이 몇 가지 인가? 이 외에 알아두어야할 것들의 리스트는 아래와 같다. 

- *linear combination* of a set of vectors
- *span* of a set of vectors

- *inear independence* of a set of vectors

- matrix가 *inverse를 가질 조건*

- *norm*

- *symmetry transformation*

- two vectors are *orthogonal*

- *orthogonal matrix*

- *Eigendecomposition*
  $$
   \mathbf{A} = \mathbf{Q}\Lambda\mathbf{Q^\top}
  $$
  모든 matrix를 eigendecompose할 수 있는 것은 아니다. (nxn) 정방행렬이 n 개의 독립인 eigenvector을 가져야 할 수 있다.  eigenvalue를 허수로 갖는 둥 복잡하게 eigendecompose되는 경우도 있다. 다행히 딥러닝에서 주로 사용되는 건 **real symmetric matrix**에 대한 eigendecomposition이다. (nxn) real symmetric matrix는 n개의 eigenvalue(중근이 있을 수도 있다)를 가지며, 그에 대응하는 eigenvector들은 orthogonal하다. Real symmetric matrix의 경우 $f(\mathbf{x}) = \mathbf{x}^\top\mathbf{A}\mathbf{x} \text{ subject to } ||\mathbf{x}||_2 = 1$이라는 quadratic expression을 최적화하는 데 eigendecomposition이 사용되기도 한다. $\mathbf{x}$이 eigenvector일 때 함수 값은 대응하는 eigenvalue이며, 위 함수의 최소 최대 값은 각각 최소 eigenvalue, 최대 eigenvlaue이다.  

  > A matrix $\mathbf{A}$ is positive definite if $\mathbf{x}^\top\mathbf{A}\mathbf{x} > 0 \text{ for any }\mathbf{x} \neq \mathbf{0}$ 
  >
  > Positive definite matrix guarantee that $\mathbf{\forall\mathbf{x}}, \mathbf{x}^\top\mathbf{A}\mathbf{x}=0 \rightarrow \mathbf{x}=\mathbf{0}$

  Eigenvalue가 모두 positive하면 그 행렬은 **positive definite**이다. 정의는 위와 같지만, eigenvalue를 통해 positive definite여부를 알아낼 수 있다. Postive definite과 같은 성질은 이후에 optimization과정에서 요긴하게 쓰인다. 

  또한, eigenvalue가 하나라도 0이면 그 행렬은 **singular**이다.

- *Singular Value Decomposition*

  Eigendecompostion은 square matrix에 대해서만 정의되는 반면, SVD는 더 일반적인 matrix에도 적용이 가능하다. 

- *The Moore-Penrose Pseudoinverse*
  $$
  \mathbf{A}^+ = \lim_{a \to 0}(\mathbf{A^\top A}+\mathbf{\alpha I})^{-1} \\
  \mathbf{A}^+ = \mathbf{VD^+U^\top}
  $$
  정의는 위와 같지만 컴퓨터를 이용해서 계산할 때는 아래와 같이 singular value decomposition을 통해 pesudoinverse matrix를 구한다.

  $\mathbf{Ax} = \mathbf{y}$에서 $\mathbf{A}$의 column이 row보다 많을 때, 즉 equation 개수보다 변수의 개수가 더 많을 때 psedoinverse를 사용하여 위 등식을 풀면, 무수히 많은 답 중 하나를 찾아준다. 단순히 '무수히 많다'에서 끝나는 게 아니고 값을 찾아주니까 의미가 있다. column보다 row가 많은 상황(해 없음)에서도 답을 찾아주는데, 그 값은 유클리디안 거리 기준으로 $\mathbf{Ax}$가 $\mathbf{y}$와 가장 가까워지는 $\mathbf{x}$ 값이다.  

