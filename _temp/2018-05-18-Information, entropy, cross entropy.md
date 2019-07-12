---
layout: post
title: "Information, entropy, cross Entropy"
description: "딥러닝에서 크로스 엔트로피가 나오는 배경은?"
comments: true
categories: [ML/수학/통계]
tags: 
- Probability
- Information Theory	
---

From Deep Learning Book, Jimin Sun's Blog



## Information of an event

정보량는 **사건**에 대해서 정의된다.    
$$
I(x) = -\log{P(x)} \\
x : \text{an event}
$$
log 를 붙여주는 이유는 엔트로피간 더하기 연산이 가능하게 만들기 위함이고,

\- 를 붙여주는 이유는 확실한 사건일수록 정보량이 적게하기 위해서다.



## Entropy

엔트로피는 **확률변수**에 대해서 정의된다  
$$
\begin{align}
\ H(X) &= \mathbb{E}_{X \sim p(X)}{[I(x)]} \\
\ &=  \sum_{i}{-P(x_i)\log{P(x_i)}} \\
\end{align} \\
$$
이것의 의미는 확률변수 X가 가지는 불확실성의 정도! X가 가지는 값이 뻔할 때 크로스 엔트로피 값은 작도록 설정되어있다. 




## Cross Entropy

크로스 엔트로피는 **두 확률밀도함수**에 대해서 정의된다.

$$
\begin{align}
\ H(P,Q) &= \mathbb{E}_{X \sim p(X)}{[-\log{Q(x)}]} \\
\ &= \sum_{i}{-P(x_i)\log{Q(x_i)}} \\
\end{align}
$$

확률 변수 X는 P(X)를 따르지만 P(X)를 모를 때 우리가 임의로 Q(X)를 따른다고 가정한다. 그랬을 때 X의 정보량을 크로스 엔트로피라고 한다. 내 가정이 틀릴수록 크로스 엔트로피 값은 증가하게 된다.

내 가정에 따라 증가한 엔트로피(정보의 불확실성)는 다음과 같이 표현할 수 있다.


$$
H(P,Q) - H(X) = \sum_{i}P(x_i)\log{\frac{P(x_i)}{Q(x_i)}} \\
\text {this is }KL(P||Q)
$$


식을 정리하고 보니까 가정에 따라 증가한 엔트로피는 KL-divergence라는 통계치로 정리된다. 이는 두 확률분포의 차이를 나타내는 통계치이다. 

**$Q(X)$를 조정함으로써 크로스엔트로피를 최소화한다**는 것은 결국 $KL(P\|\|Q)$값을 최소화한다는 것과 동치이다!


$$
KL(P||Q) =\mathbb{E}_{X \sim p(X)}{[\log{\frac{P(x)}{Q(x)})}]}  = \mathbb{E}_{X \sim p(X)}{[\log{P(x)}-\log{Q(x)}]}
$$





## TODO:

MLE is minimizing KL div between model dist and empirical dist - > minimizing negative log likeihood

Minimizing KL is minimizing cross entropy.

Therefore, minimizing NLL is same as minimizing crossentropy

[참고](https://taeoh-kim.github.io/blog/cross-entropy의-정확한-확률적-의미/)

