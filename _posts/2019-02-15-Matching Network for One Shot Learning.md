---
layout: post
title: "Matching Network for One shot Learning"
description: "Meta-learning 논문 리뷰"
comments: true
categories: [NLP]
tags:
- Meta-learning

---



Matching Networks for One shot Learning by Vinayls et al., 2016. NIPS.



## Preliminary

- Few-shot learning: 적은 양의 데이터로부터 학습하기. 사람은 잘 하지만 딥러닝은 못하는 것의 대표적인 예시로 쓰인다.
- N-way, k-shot learning task: few-shot learning의 성능을 측정하는 데 사용되는 대표적인 task. 학습시 주어지지 않은 class를 가지는 test example을 얼마나 잘 분류하는지 평가하는 task이다. 이때, classification은 N-way 분류 문제이고, 각 class마다 k개의 example들이 주어진다.  



## What

One-shot learning을 하는 모델과 training procedure을 제안한다. Deep learning은 많은 분야에서 뛰어난 성능을 보이고 있지만 엄청난 양의 데이터를 필요로 하는 단점이 있다. 이 논문은 그 원인이 딥러닝의 parametric한 측면, 즉 parameter들이 천천히 train example을 배워가는 데 있다고 본다. 제안모델은 attention을 사용하는 memory network로 classification 과정에서 파라미터가 필요없다는 특징을 가진다. Training procedure은 train시와 test시의 환경이 동일하도록 구성된다.



## How

제안 모델은 one-shot learning을 set-to-set framework의 변형으로 설명한다. K개의 example 및 그 label로 이루어진  support set $$S=\{(x_i, y_i)\}$$를 classifier $$C_S(\hat{x})$$로 mapping시키는게 목표이다. 이 classifier는 새로운 example $$\hat{x}$$가 주어졌을 때 어떤 class에 속하는지에 대한 확률분포 $$\hat{y}$$를 뱉어낸다.
$$
\hat{y} = \sum_{i=1}^{k}a(\hat{x}, x_i)y_i
$$
 즉, test example의 label은 주어진 support set의 label의 조합으로 구성된다. $$y_i$$는 memory로 본다면 이 과정은 attention augmented memory network로 이해할 수 있다.

Attention은 임베딩된 두 이미지에 대해서 계산된다. Support set의 example에 대한 Embedding function을 $$g(x_i)$$로 구성할 경우 support set내 각 이미지들은 서로에 대해 independent하게 벡터로 변환된다. 이로 인한 문제점은 support set사이에 유사하지만 다른 class를 갖는 이미지가 있더라도(e.g. 삽살개, 진돗개) $$g$$는 두 이미지에 대해 유사한 벡터값을 부여한다는 것이다. 만약 embedding function을 $$g(x_i, S)$$로 구성한다면 이를 해결할 수 있다. 주어진 support set을 고려해서 test example에 대한 임베딩을 만들 필요도 있는데, 이는 $$f(\hat{x}, S) =attnLSTM(f'(\hat{x}, g(S), K) $$와 같은 함수를 통해 실현 가능하다.

Train과 test때 동일한 환경을 만들기 위한 목적함수는 아래와 같이 구성된다.
$$
\theta = argmax_{\theta}\mathbb{E}_{L \sim T}\Big[\mathbb{E}_{S\sim L, B \sim L}\Big[\sum_{(x,y)\in B}\log{P_\theta(y|x, S)}\Big]\Big]
$$


## Notes

- Memory network를 one-shot learning에 이용했다. Memory는 support set의 label들이며, 이 memory를 찾아오기 위해 attention을 사용한다.

- 