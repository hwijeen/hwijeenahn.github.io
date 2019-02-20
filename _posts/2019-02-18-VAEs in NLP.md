---
layout: post
title: "VAEs in NLP"
description: "NLP 문제에 VAE기반 모델을 적용한 논문들"
comments: true
categories: [NLP]
tags:
- VAE
---



## VAE? CVAE? why not seq2seq?

CVAE는 VAE에서 발전된 모델이다. 두 모델 모두 generation 모델이지만, VAE는 무엇을 만들어낼 것이냐를 제어할 수 없다. CVAE는 대화 기록이 주어지고 다음 발화를 생성해라, 문장이 주어지고 paraphrase를 만들어내라와 같이 목적이 있는 문장 생성을 할 수 있다. [VAE 개념 참고 블로그 글](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/), [A Tutorial on Deep Latent Variable Models of Natural Language](https://arxiv.org/abs/1812.06834)에서 VAE의 training & test 과정 및 구조를 알아야 CVAE 이해가 가능하다. 

VAE기반 모델의 장점은 one-to-many mapping이 가능하다는 것이다. 기존의 sequence to sequence 모델은 주어진 입력에 대한 deterministic representation을 만들고 이를 기반으로 디코더가 문장을 생성한다. VAE는 주어진 입력에 대한 representation을 확률분포로 정의하기 때문에, 샘플링을 통해 다양한 문장을 생성할 수 있다. 

이 포스트에서 정리할 논문들은 아래와 같다.

- [Sohn et al., 2015](https://papers.nips.cc/paper/5775-learning-structured-output-representation-using-deep-conditional-generative-models.pdf): CVAE를 처음 제안. 이론적 배경이 탄탄하다.
- [Bowman et al., 2015](https://arxiv.org/abs/1511.06349): VAE를 NLP에 처음 적용한 논문. Training difficulty에 대한 대한 대처법을 잘 고안했다.

- [Zhao et al., 2017](https://arxiv.org/abs/1703.10960): CVAE를 dialog에 적용했다. 
- [Gupta et al., 2018](https://arxiv.org/abs/1709.05074): CVAE를 사용해 paraphrase generation을 했다.



## Learning Discourse-level Diversity for Neural Dialog Models Using Conditional Variational Autoencoders

#### WHAT

기존의 encoder-decoder기반 대화 모델은 일반적이고 뻔한 대답을 내뱉는다는 문제가 있다. 이 문제를 해결하기 위해서 CVAE 기반의 모델을 제안한다. 이 모델의 아이디어는 대화를 one-to-many 문제로 보자는 것이다. Dialog level의 정보를 잡아내는 latent variable을 도입함으로써 다양한 발화를 생성할 수 있는 모델을 개발했다. 

#### HOW: modeling

대화를 다음 세 가지 확률 변수로 모델링한다. Dialog context $c$, response utterance $x$, latent variable $z$. 이러한 확률 변수를 가지고 정의하는 분포는 아래와 같다.
$$
p(x, z|c) = p(z|c)p(x|z,c)
$$
뉴럴넷의 용어로 표현하면 $p(z|c)​$는 (conditional) **prior network**, $p(x|z,c)​$는 **response decoder**이다. 뉴럴넷 파라미터는 $p(x,z|c)​$를 최대화하도록 학습된다. 논문 표현에 따르면 이 방법은 maximizing the conditional likelihood of x given c이다. 통계 문헌에서 얘기하는 파라미터 추정 방법중 하나인 marginal likelihood of -parameter를 최대화하는 방법을 일컫는 것 같다.($p(x|c) = \int p(x|z,c)p(z|c)dz​$)

Test time때는 prior network에서 $z​$를 sampling한 뒤, 이를 response decoder에 집어넣어 대화를 생성해낸다. 이때, sampling한 $z​$값에 따라 생성되는 대화가 달라진다. **One-to-many**!

#### How: training

$p(x|c)​$를 직접 optimize하는 건 intractable하기 때문에 이의 lower bound = ELBO = variational enery를 objective function으로 삼아 학습을 한다. Approximation하는 거다. 통계 용어로 variational distribution = 뉴럴넷 용어로 **recognition network** $q(z|x,c )​$ 를 도입하면 variational lower bound 식을 도출할 수 있다. 여기서 $q(z|x,c)​$와 $p(z|c)​$가 정규분포를 따른다고 가정하면 아래 식을 깔끔하게 계산까지 할 수 있다.
$$
P(x|c) \geq \mathbb{E}_{q(z|x,c)}{p(x|z,c)} - KL(q(z|x,c) || p(z|c))
$$
Objective function을 최대화 하는 과정을 말로 해석하면 첫번째 term은 recognition network에서 샘플링한 $z$로 계산한 likelihood를 높이기, 두 번째 term은 recognition network가 prior network와 비슷하도록 하기 정도겠다. 이는 한 단계 위에서(?) 보면 recognition network로 posterior distribution를 근사하는 과정으로 해석할 수 있다[(해석 설명)](https://www.edwith.org/bayesiandeeplearning/lecture/25284/).  

>  VAE가 CVAE로 진화하면서 생긴 차이점은 prior가 conditional distribution이 된다는 점이다. VAE에서 prior distribution은 고정이었지만 CVAE에선 condition으로 무엇이 주어지냐에 따라 바뀌게 되고, 파라미터 추정의 대상이 된다. 

조건부 확률분포를 뉴럴넷으로 표현할 때 사용하는 방법들: Utterance encoder는 BiGRU, context encoder는 GRU, response decoder은 GRU이다. Encoder가 내뱉는 $\sigma, \mu​$로부터 sampling(through reparameterization trick)된 벡터를 response decoder의 initial hidden state로 삼음으로써 $(x\|z, c)​$라는 condition을 모델링한다.

이 방법은 vanishing latent variable problem문제가 발생한다. Decoder를 RNN계열로 구성해버리면 backprop시 encoder까지 loss가 잘 전파되지 않고, 결국 encoder가 $z$에 유용한 정보를 encode하지 못하는 상황을 일컫는 것 같다.  VAE를 text에 처음 적용한 [Bowman et al.,2015](https://arxiv.org/pdf/1511.06349.pdf)에서 이 문제에 대한 해결책을 제시한 바 있지만 여기서는 또 다른 해결책을 제시한다. Bag-of-word loss를 도입하는 것이다. Encoding된 $z$가 순서를 보존한 원래 문장을 복원할 뿐만 아니라 bag-of-word까지 맞춰야하는 task까지 풀게 하는 것이다.
$$
p(x,z|c) = p(x_{original}|z,c)p(x_{bow}|z,c)p(z|c)
$$

$$
\text{when } x_o \text{ and } x_{original} \text{ are conditionally indep given z,c}
$$



## A Deep Generative Framework for Paraphrase Generation

#### WHAT

Conditional generation의 일종인 paraphrase generation을 하기 위해서 CVAE를 사용했다. 기존의 sequence to sequence 모델에 비해 'principled generative framework'를 가진다는 게 장점이다. 또한, one-to-many generation을 하려면 CVAE 모델이 더 적합하다.

#### HOW

Training objective는 CVAE에서 다루는 것과 동일하다. Variational 분포에서 뽑은 $z$ 를 사용한 reconstruction loss를 최소화하면서, 그 분포가 최대한 prior에 가깝게 하는 것이다. 위 논문과 차이점은 **prior network를 unconditional distribution으로 사용한다는 것**! CVAE를 처음 제안한 Sohn et al.,에 따르면 이는 선택 사항이다.

>  The prior of the latent variable $z$ is modulated by the input $x$ in our formulation; however, the constraint can be easily relaxed to make the latent variables statistically independent of input variables, i.e., $p_\theta(z\|x) =p_\theta(x)$

$$
\mathcal{L}(\theta, \phi; x^p, x^o) = \mathbb{E}_{q_{\phi}(z|x^o, x^p)}[\log{p_\theta(x^p | x^o, z)}] - KL(q_{\phi}(z|x^o, x^p)||p(z))
$$

![VAE-LSTM](\assets\img\gupta2018.PNG)

Recognition network $q_\phi(z\|x^o, x^p)​$ 표현에는 두 개의 LSTM을 사용한다. 첫 번째 LSTM은 $x^o​$을 인코딩하고, 마지막 hidden state를 두 번째 LSTM으로 넘긴다. 두 번째 LSTM은 $x^p​$을 인풋으로 받는다. 마지막 hidden state는 MLP를 통과해 $q_\phi(z\|x^o, x^p)​$의 파라미터를 내뱉는다. Decoder에서도 동일한 방식으로 두 개의 LSTM이 사용된다. Reparameterization trick을 통해 recongition network에서 sampling된 $z​$는 decoder의 두 번째 LSTM($x^p​$를 reconstruction 하는 LSTM)에서 매 time step 입력으로 들어간다. (Encoder와 decoder 모두에 $x^o​$인코더가 존재하는 셈인데, 이를 다른 LSTM으로 구성했을 때 성능이 더 좋았다고 한다.)

- Generation LSTM의 input으로 $z​$가 어떻게 들어가지? $w_{e1}^p​$도 들어가야할텐데? 더 할까 concat할까

