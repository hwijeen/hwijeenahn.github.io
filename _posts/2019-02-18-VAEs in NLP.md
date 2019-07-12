---
layout: post
title: "VAEs in NLP"
description: "NLP에서 VAE기반 모델을 사용하는 논문들"
comments: true
categories: [논문 리뷰]
tags:
- VAE
- CVAE
---



## VAE? CVAE? why not seq2seq?

VAE기반 모델의 장점은 one-to-many mapping이 가능하다는 것이다. 기존의 sequence to sequence 모델은 주어진 입력에 대한 deterministic representation을 만들고 이를 기반으로 디코더가 문장을 생성한다. VAE는 주어진 입력에 대한 representation을 확률분포로 정의하기 때문에, 샘플링을 통해 다양한 문장을 생성할 수 있다. CVAE는 VAE에서 발전된 모델이다. 두 모델 모두 generation 모델이지만, VAE는 무엇을 만들어낼 것이냐를 제어할 수 없다. CVAE는 대화 기록이 주어지고 다음 발화를 생성해라, 문장이 주어지고 paraphrase를 만들어내라와 같이 목적이 있는 문장 생성을 할 수 있다. [VAE 개념 참고 블로그 글](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/), [A Tutorial on Deep Latent Variable Models of Natural Language](https://arxiv.org/abs/1812.06834), [Tutorial on Variational Autoencoders](https://arxiv.org/abs/1606.05908), [매우 잘 정리된 블로그](http://ruishu.io/2018/03/14/vae/)에서 개념을 참고했다.

이 포스트에서 정리할 논문들은 아래와 같다.

- [Sohn et al., 2015](https://papers.nips.cc/paper/5775-learning-structured-output-representation-using-deep-conditional-generative-models.pdf): CVAE를 처음 제안. 이론적 배경이 탄탄하다.
- [Bowman et al., 2015](https://arxiv.org/abs/1511.06349): VAE를 NLP에 처음 적용한 논문. Training difficulty에 대한 대한 대처법을 잘 고안했다.

- [Zhao et al., 2017](https://arxiv.org/abs/1703.10960): CVAE를 dialog에 적용했다. 
- [Gupta et al., 2018](https://arxiv.org/abs/1709.05074): CVAE를 사용해 paraphrase generation을 했다.



## VAE: motivations and ideas

Generative 모델은 $p(x)$를 찾는 것을 목표로 한다. $p(x) $를 알고있다면 sampling을 통해 data를 만들어낼 수 있기 때문에 generation model이라고 할 수 있다. 일반적으로 generation model은 1 strong assumption about the structure in data, 2 severe approximation, 3 rely on computationally expesive inference procedure가 단점으로 꼽힌다. VAE의 장점은 이러한 단점을 극복하는 generation 모델이라는 것이다. VAE는 data structure에 대한 가정이 적고 backpropagation을 통한 빠른 훈련이 가능하다. VAE도 물론 approximation을 사용하지만 그 error가 적다. 

VAE는 latent variable model이다. Latent variable $z​$가 도입된 VAE를 plate notation으로 표현하면 다음과 같다. [!그림 - x 하나마다 z하나씩 가지고 있음](). 이러한 latent variable model은 실제 data를 만들어내는 process를 더 잘 표현할 수 있다는 장점이 있다. 통계적으로 볼땐, 하나의 variable을 표현하는 distribution보다 더 complext한 distribution을 표현할 수 있다.

> Maching learning의 궁긍적인 목표는 train data와 test data뿐만 아니라 data 전체를 만들어내는 true data distribution $p^*(x)​$를 찾는 것이다. 그런데 우리가 가진 건 여기서 나온 몇 개의 sample들(train data)뿐이다. 이런 상황에서 우리가 할 수 있는 건 이 sample들이 구성하는 empirical distribution $\hat{p}(x)​$를 찾는 것이다. 찾는다는 것은 여러 개의 후보 distribution set을 구성하고  empirical distribution $\hat{p}(x)​$과 가장 가까운 $p(x)​$를 고르는 과정으로 이해할 수 있다. 가깝다 멀다 기준을 KL divergence로 잡는 경우가 MLE estimation을 하는 것이다. 

$$
p(x) = \int p(x|z;\theta)p(z)dz
$$

Latent variable을 도입해서 likelihood를 위와 같이 전개하면 두 가지 문제를 해결해야한다. 1 $p(z)$를 어떻게 설정해야하나? 2 integral을 어떻게 처리하나? 첫번 째 문제에 대한 VAE의 답은 unit gaussian으로 두자는 것이다. 그 이유는 any d-dimensional distribution can be approximated with a set of sampels from d-dimensional normal distribution. Universal functiona approximator인 neural network를 이용하면 unit gaussian에서 뽑은 sample들을 실제 우리가 필요한 latent variable들로 변환시킬 수 있다(VAE의 decoder $p(x|z)​$에서 초기 layer가 이러한 역할을 담당한다고 이해할 수 있다).

Continuous variable $z$에 대한 integral은 computationally intractable하다. 그렇기 때문에 VAE는 integral을 포함하는 $p(x)$대신 계산 가능한 $p(x)$의 $ELBO$를 optimize하는 approximation방식을 선택한다. $p(x)$를 전개할 때 임의의 분포 $q(z|x)$를 도입하면 $ELBO$가 포함된 식을 전개할 수 있다. 이러한 $ELBO$를 optimize하는 과정은 variational inference를 수반한다. $ELBO$ objective를 들여다보면 autoencoder 구조가 녹아있음을 볼 수 있다.

> space of $z$ from $q(z|x)$ should be smaller than that of $z$ from $p(z)$, so that expectation in $\mathbb{E}_{z \sim q(z|x)}[p(x|z)]$ is easily approximated with a few samples of $z$.

- ELBO 식 전개
- Reparameterization(conditional form of parametric distribution 만들기, gradient expectation안으로 넣기)

## VAE vs. CVAE

CVAE와 VAE는 1) variational distribution, 2) prior distribution, 3)decoder network를 어떻게 구성하냐에서 차이를 보인다. CVAE에서 variational distribution은 $q_\phi(z|x, y)​$로 표현된다. 이때 $x, y​$는 데이터의 한 쌍을 의미한다. 여기서 중요한 점은 **$z​$에 대한 variational distribution이 $x​$뿐만 아니라 $y​$에까지 conditioning된다는 것**이다. CVAE는 conditional marginal likelihood $p(y|x)​$를 최대화하는 방향으로 훈련이되는데, 이는 variatonal distribution $q_\phi(z|x, y)​$가 posterior distribution $p(z|x, y)​$에 근사되는 결과를 가져온다. 데이터가 $x, y​$  쌍으로 주어졌으니 때 posterior의 condition으로 $x, y​$가 들어가게되고, variational distribution은 posterior을 근사하기 위한 용도이니까 똑같이 $x, y​$에 대해 condition된다고 이해하면 맞는 걸까?

CVAE의 decoder network는 $p_\theta(y|z, x)$로 표현되고, VAE와의 차이는 **decoder network가 original sentence $x$에 대해서도 conditioning된다는 것**이다. 이 차이 덕분에 test time때 input specific한 generation이 가능해지는 것이다. 





## TODO

- VAE 식 전개
- What is bayesian in VAE?
- cs231n이 설명하는 VAE에서 p(z|x)를 고려하는 이유?
- test time때 p(z)에서 sampling하는 것의 근거? 