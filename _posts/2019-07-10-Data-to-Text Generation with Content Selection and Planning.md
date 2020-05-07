---
layout: post
title: "Data-to-Text Generation with Content Selection and Planning"
description: "Data-to-Text 관련 논문 리뷰"
comments: true
categories: [Paper]
tags:
- Data-to-text
- Rotowires
---

Data-to-Text Generation with Content Selection and Planning., AAAI 2019. [paper](https://arxiv.org/abs/1809.00582)

## Idea
이 논문은 RotoWire dataset을 이용한 data-to-text task 성능을 높이기 위한 새로운 모델을 제시한다. 저자는 data-to-task를 1) content plan과 2) text generation 두 부분으로 세분화해서 접근한다. 제안 모델도 unordered record로부터 content plan을 만드는 부분과 주어진 content plan에 기반해 자연어 summary를 만들어내는 부분으로 구성된다. 두 개의 submodule로 접근하는 전통적인 방법을 neural network를 사용한 end-to-end 학습에 적용한 것이다. 

  

## Model
$$
p(y|r) = \sum_z p(y,z|r) = \sum_z p(z|r) p(y|r,z)
$$

Records $r$에 따른 summary $y$의 확률 모델은 $p(z|r)$과 $p(y|r,z)$ 두 부분으로 쪼개진다. 각각의 확률 분포는 Encoder-Decoder로 모델링된다.

### 1. $p(z|r)$

***Encoder-Decoder***
    ***Embedding***:  simple lookup table + *self-attention(?)*  
    ***Encoder***: 별다른 처리 없이 embedding을 쌓아두기  
    ***Decoder***:  RNN  
    ***Generator***:  Pointer network  

#### Record Encoder

$$
\mathbf{r}_j = ReLU(W_r[r_{j,1}; r_{j,2}; r_{j,3}; r_{j,4}])
$$

Raw text를 input으로 받아서 이에 대한 vector representation $r_j$을 만들어준다. record에 포함된 type, entity, value, home/away 네 가지 항목을 simple lookup-table을 통해 임베딩 한 뒤 MLP을 거쳐 하나의 벡터로 합친다.

#### Content Selection Gate

하나의 record representation $r_j$를 만들 때 주변 record $r_{i \neq{j} }$를 고려하면 더 풍부한 정보를 담을 수 있다는 아이디어다. 예를 들어, '한 선수(entity)가 고(value)득점(type)을 했다'는 record의 표현을 만들 때 이 선수의 rebound정보, 3점슛 개수 등을 반영하면 summary를 만들 때 더 도움이 된다는 것이다. Self-attention mechanism 활용해서 주변 정보를 반영한다.
$$
\alpha_{j,k} \propto exp(r_j^\intercal W_a r_k) \\
c_j = \sum_{k \neq j} \alpha_{j,k} r_k \\
r_j^{att} = W_g[r_j; c_j]
$$
$r_j^{att}$에는 j번 째 record에 대한 정보($r_j$)와 주변 정보($c_j$)가 모두 담겨있다. 우리가 원하는 건 전체 record에 대한 정보가 아니라 j번 째 record에 대한 representation고, 이때 context정보를 일정부분 고려하자는 것이다. 이를 위해 context mechanism을 활용한다.
$$
g_j = sigmoid(r_j^{att}) \\
r_j^{cs} = g_j \odot r_j
$$
$r_j^{cs}$가 j번 째 record에 대한 최종 representation, 즉 $p(z|r)$ 모델의 인코더의 output이다.

  

#### Content Planning

$$
p(z|r) = \prod_{k=1}^{|z|} p(z_k | z_{<k}, r)
$$



$p(z|r)$모델의 디코더로서, 인코딩된 record representation을 입력으로 받아서 content sequence에 대한 확률 분포를 autoregressive한 방식으로 내뱉는다. 이때 content($z_k$) sequence는 input으로 들어온 여러 개의 record에 순서를 부여한 것이다. 매 time step에서 source side에 입력으로 들어온 여러 record중 하나의 record를 고르는데 pointer network가 사용된다. 
$$
p(z_k= r_j|z_{<k}, r) \propto exp(h_k^{\intercal} W_c r_j^{cs})
$$


  - first hidden으로는$r_j^{cs}$의 평균을 사용한다.

  * 이와 같은 모델을 훈련하려면 정답 planning sequence가 필요한데, 이는 별도의 information extraction module을 통해서 얻어낸다.

    

  ### 2. $p(y|r,z)$

  **Encoder-Decoder**
      ***Embedding***:  simple look-up table  
      ***Encoder***: Bidirectional RNN
      ***Decoder***:  RNN  
      ***Generator***:  Copy mechanism(joint copy or conditional copy)

  #### Text Generation

  $p(z|r)$ 모듈을 통해 content plan sequence가 만들어졌으면, 이에 condition해서 자연어 text를 만들어낸다. Encoder-decoder모델의 source side(bidrectional LSTM)에 input으로 들어가는 것은 순서가 정해진 record sequence이며, decoder는 autogressive한 방식으로 텍스트를 만들어낸다. Attention mechanism과 copy mechanism을 통해 성능을 향상시켰다. 
$$
\beta_{t,k} \propto exp(d_t^{\intercal}W_be_k) \\
  q_t = \sum_{k} \beta_{t,k} e_k \\
  d_t^{att} = tanh(W_d[d_t;q_t]) \\
  p_{gen}(y_t|y_{<t}, z, r) = \sum_{u_t \in\left\{0, 1\right\}}p(y_t, u_t | y_{<t}, z, r)
$$

  


  ## Experiment
  ![result](/assets/img/puduppully_result.png)

  

  ## Issue

  - Content Selection gate에서 사용하는 게 Self-attention 맞습니까?
  - Pointer network를 통해 source side에 대한 distribution을 만들텐데, 이후에 source token중에서 하나로 특정하는 건 argmax? sampling?

