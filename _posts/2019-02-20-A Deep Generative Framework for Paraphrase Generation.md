---
layout: post
title: "A Deep Generative Framework for Paraphrase Genertion"
description: "VAE 관련 논문 리뷰"
comments: true
categories: [논문 리뷰]
tags:
- VAE
- CVAE
---

LearA Deep Generative Framework for Paraphrase Generation,  Gupta et al., ACL 2017. [paper](https://arxiv.org/abs/1709.05074) [my implementation](https://github.com/hwijeen/VAE-LSTM)



## WHAT

Conditional generation의 일종인 paraphrase generation을 하기 위해서 CVAE를 사용했다. 기존의 sequence to sequence 모델에 비해 'principled generative framework'를 가진다는 게 장점이다. 또한, one-to-many generation을 하려면 CVAE 모델이 더 적합하다.



## HOW

Training objective는 CVAE에서 다루는 것과 동일하다. Variational 분포에서 뽑은 $z​$ 를 사용한 reconstruction loss를 최소화하면서, 그 분포가 최대한 prior에 가깝게 하는 것이다. 위 논문과 차이점은 **prior network를 unconditional distribution으로 사용한다는 것**! CVAE를 처음 제안한 Sohn et al.,에 따르면 이는 선택 사항이다.

> The prior of the latent variable $z$ is modulated by the input $x$ in our formulation; however, the constraint can be easily relaxed to make the latent variables statistically independent of input variables, i.e., $p_\theta(z\|x) =p_\theta(x)$

$$
\mathcal{L}(\theta, \phi; x^p, x^o) = \mathbb{E}_{q_{\phi}(z|x^o, x^p)}[\log{p_\theta(x^p | x^o, z)}] - KL(q_{\phi}(z|x^o, x^p)||p(z))
$$

![VAE-LSTM](/assets/img/gupta2018.PNG)

Recognition network $q_\phi(z\|x^o, x^p)​$ 표현에는 두 개의 LSTM을 사용한다. 첫 번째 LSTM은 $x^o​$을 인코딩하고, 마지막 hidden state를 두 번째 LSTM으로 넘긴다. 두 번째 LSTM은 $x^p​$을 인풋으로 받는다. 마지막 hidden state는 MLP를 통과해 $q_\phi(z\|x^o, x^p)​$의 파라미터를 내뱉는다. Decoder에서도 동일한 방식으로 두 개의 LSTM이 사용된다. Reparameterization trick을 통해 recongition network에서 sampling된 $z​$는 decoder의 두 번째 LSTM($x^p​$를 reconstruction 하는 LSTM)에서 매 time step 입력으로 들어간다. (Encoder와 decoder 모두에 $x^o​$인코더가 존재하는 셈인데, 이를 다른 LSTM으로 구성했을 때 성능이 더 좋았다고 한다.)

- Generation LSTM의 input으로 $z$가 어떻게 들어가지? $w_{e1}^p$도 들어가야할텐데? 더 할까 concat할까

