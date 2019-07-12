---
layout: post
title: "Pretraining-Based Natural Language Generation for Text Summarization"
description: "Pretrained LM을 이용한 generation model"
comments: true
categories: [논문 리뷰]
tags:
- Data-to-text
- Rotowires
---

## Idea

***한 줄 요약***: Pretrained LM BERT를 이용하는 NLG 모델(encoder-decoder)을 제안한다. 

Decoding은 two stage로 진행되는데, first stage에서 transformer decoder가 draft output sequence를 만들어낸다. 그런 다음 준비 단계로 이 Draft output의 하나의 단어를 mask처리한 뒤 BERT를 통해 인코딩한다.  Second stage는 original input sequence와 masked draft output을 입력으로 받아 mask된 단어에 대한 예측을 한다. Second stage를 draft output의 모든 단어에대해 반복하는 것이 refined output을 만들어내는 과정이다.

  

## Difference

기존의 NLG모델의 decoder는 1) left-context-only(autoregressive)를 기반으로 한다는 점, 2)pre-trained contextualied LM을 사용하지 못한다는 한계를 가지는데, 이 두 가지를 극복하는 모델을 제안했다.

  

## Model

전체 모델은 input document $X = \{x_1, ..., x_m\}$을 summary $Y= \{y_1, ..., y_L\}$로 바꾸는 모델이다. 

Summary **draft** $A = \{ a_1, ..., a_{|a|}\}$는 BERT 인코더와 transformer decoder를 사용해서 만든다. BERT 인코더 output $H=BERT(x_1, ..., x_m)$는 document $X$의 representation으로 사용된다. BERT 인코더 내부의 word embedding matrix output은 $\{q_1, ..., q_m\}$이라고 한다. Transformer decoder는 매 time step마다 단어를 생성해낸다. *이때 embedidng matrix는 encoder와 공유한다*. Summarization에서는 source sequence의 단어를 그대로 가져와야하는 상황이 많기에 copy mechanism을 도입한다. 
$$
P_t(w) = (1-g_t)P_t^{vocab} + g_t\sum_{i:w_i=w}\alpha_t^i \\
g_t = sigmoid(W_g \odot [o_t;h] + b_g) \\
o_t = \sum_j \alpha_t^j h_j\\
\alpha_t^j = softmax(u_t^j) \\
u_t^j = o_tW_ch_j\\
P_t^{vocab}(w) = f_{dec}(q<t, H)
$$
이 과정에서 첫 번째 loss $L_{dec}$이 발생한다.
$$
L_{dec} = \sum_{t=1}^{|a|}-\log P(a_t = y_t^*, H)
$$
**Refined** summary를 만들기 위해서는 *BERT와 transformer decoder가 한 번 더 사용된다*. t번 째 time step에서 draft summary의 t번째 단어를 mask한 뒤 BERT를 사용해 인코딩한다. Decoder는 BERT  representation $H$, t번 째 단어를 제외한 모든 context의 representation $a_{\neq t}$를 기반으로 t번째 단어에 대한 예측을 다시한다. 여기서 발생하는 loss $L_{refine}$은 아래와 같다.
$$
L_{refine} = \sum{t=1}^{|y|} - \log P(y_t=y_t^*| a_{\neq t}, H)
$$
이에 따른 최종 loss는 아래와 같다(강화학습 부분은 생략). 
$$
L_{model} = L_{dec} + L_{refine}
$$


## Experiment

![result](/assets/img/zhang result.png)

CNN/Daily Mail Dataset을 가지고 실험을 했으며, 비교 모델은 크게 Extractive와 Abstractive로 나뉜다. 

  

## Issue

- BERT는 계속 fine tuning되는 건가?
- Pretrained BERT는 tokenizing이 wpm단위일텐데, down stream task에서도 이걸 똑같이 따라가야 하는 건가?
- Test time때는 beam serach로 draft summary를, greedy decoding으로 refined summary를 만들어낸다.
- 각종 후처리 기법 사용 함 - OpenNMT사용한듯?
- Reinforcement learning(policy gradient) loss 추가했음