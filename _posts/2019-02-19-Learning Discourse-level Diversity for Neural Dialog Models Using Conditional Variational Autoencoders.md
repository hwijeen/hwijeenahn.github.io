---
layout: post
title: "Learning Discourse-level Diversity for Neural Dialog Models Using Conditional Variational Autoencoders"
description: "VAE 관련 논문 리뷰"
comments: true
categories: [Paper]
tags:
- VAE
- CVAE
---

Learning Discourse-level Diversity for Neural Dialog Models Using Conditional Variational Autoencoders, Zhao et al., ACL 2017. [paper](https://arxiv.org/abs/1703.10960) 



## WHAT

기존의 encoder-decoder기반 대화 모델은 일반적이고 뻔한 대답을 내뱉는다는 문제가 있다. 이 문제를 해결하기 위해서 CVAE 기반의 모델을 제안한다. 이 모델의 아이디어는 대화를 one-to-many 문제로 보자는 것이다. Dialog level의 정보를 잡아내는 latent variable을 도입함으로써 다양한 발화를 생성할 수 있는 모델을 개발했다. 



## HOW: modeling

대화를 다음 세 가지 확률 변수로 모델링한다. Dialog context $c$, response utterance $x$, latent variable $z$. 이러한 확률 변수를 가지고 정의하는 분포는 아래와 같다.
$$
p(x, z|c) = p(z|c)p(x|z,c)
$$
뉴럴넷의 용어로 표현하면 $p(z|c)$는 (conditional) **prior network**, $p(x|z,c)$는 **response decoder**이다. 뉴럴넷 파라미터는 $p(x,z|c)$를 최대화하도록 학습된다. 논문 표현에 따르면 이 방법은 maximizing the conditional likelihood of x given c이다. 통계 문헌에서 얘기하는 파라미터 추정 방법중 하나인 marginal likelihood of -parameter를 최대화하는 방법을 일컫는 것 같다.($p(x|c) = \int p(x|z,c)p(z|c)dz$)

Test time때는 prior network에서 $z$를 sampling한 뒤, 이를 response decoder에 집어넣어 대화를 생성해낸다. 이때, sampling한 $z$값에 따라 생성되는 대화가 달라진다. **One-to-many**!

## How: training

$p(x|c)$를 직접 optimize하는 건 intractable하기 때문에 이의 lower bound = ELBO = variational enery를 objective function으로 삼아 학습을 한다. Approximation하는 거다. 통계 용어로 variational distribution = 뉴럴넷 용어로 **recognition network** $q(z|x,c )$ 를 도입하면 variational lower bound 식을 도출할 수 있다. 여기서 $q(z|x,c)$와 $p(z|c)$가 정규분포를 따른다고 가정하면 아래 식을 깔끔하게 계산까지 할 수 있다.
$$
P(x|c) \geq \mathbb{E}_{q(z|x,c)}{p(x|z,c)} - KL(q(z|x,c) || p(z|c))
$$
Objective function을 최대화 하는 과정을 말로 해석하면 첫번째 term은 recognition network에서 샘플링한 $z$로 계산한 likelihood를 높이기, 두 번째 term은 recognition network가 prior network와 비슷하도록 하기 정도겠다. 이는 한 단계 위에서(?) 보면 recognition network로 posterior distribution를 근사하는 과정으로 해석할 수 있다[(해석 설명)](https://www.edwith.org/bayesiandeeplearning/lecture/25284/).  

> VAE가 CVAE로 진화하면서 생긴 차이점은 prior가 conditional distribution이 된다는 점이다. VAE에서 prior distribution은 고정이었지만 CVAE에선 condition으로 무엇이 주어지냐에 따라 바뀌게 되고, 파라미터 추정의 대상이 된다. 

조건부 확률분포를 뉴럴넷으로 표현할 때 사용하는 방법들: Utterance encoder는 BiGRU, context encoder는 GRU, response decoder은 GRU이다. Encoder가 내뱉는 $\sigma, \mu$로부터 sampling(through reparameterization trick)된 벡터를 response decoder의 initial hidden state로 삼음으로써 $(x\|z, c)$라는 condition을 모델링한다.

이 방법은 vanishing latent variable problem문제가 발생한다. Decoder를 RNN계열로 구성해버리면 backprop시 encoder까지 loss가 잘 전파되지 않고, 결국 encoder가 $z$에 유용한 정보를 encode하지 못하는 상황을 일컫는 것 같다.  VAE를 text에 처음 적용한 [Bowman et al.,2015](https://arxiv.org/pdf/1511.06349.pdf)에서 이 문제에 대한 해결책을 제시한 바 있지만 여기서는 또 다른 해결책을 제시한다. Bag-of-word loss를 도입하는 것이다. Encoding된 $z$가 순서를 보존한 원래 문장을 복원할 뿐만 아니라 bag-of-word까지 맞춰야하는 task까지 풀게 하는 것이다.
$$
p(x,z|c) = p(x_{original}|z,c)p(x_{bow}|z,c)p(z|c)
$$

$$
\text{when } x_o \text{ and } x_{original} \text{ are conditionally indep given z,c}
$$

