---
layout: post
title: "Challenges in Data-to-Document Generation"
description: "VAE 관련 논문 리뷰"
comments: true
categories: [논문 리뷰]
tags:
- VAE
- CVAE
---

Challenges in Data-to-Document Generation, Wiseman et al., EMNLP 2017. [paper](https://arxiv.org/abs/1707.08052)

## Idea
논문의 핵심은 1) 새로운 data-to-text generation task를 제안, 2) baseline 모델 제안, 3) RotoWire dataset 배포, 4)  extractive evaluation 방법을 제안한 것이다. Table과 같은 structured data에서 natural language를 만들어 내려면 1) what to say, 2) how to say 두 가지 문제를 해결해야하는데, 이를 위해 neural network 기반의 end-to-end 모델을 사용한다. 제안한 여러 가지 Baseline 중 가장 성능이 높았던 것은 Encoder-Decoder + copy mechanism 모델에 additional reconstruction loss를 사용한 방법이다.			

  

## Model
***Encoder-Decoder***{
    ***Embedding***:  simple look-up table  
    ***Encoder***: 별다른 처리 없이 embedding을 쌓아두기  
    ***Decoder***:  RNN  
    ***Generator***:  Copy mechanism(joint copy or conditional copy)
    }

#### Copy mechanism

사용된 모델은 기존에 제안된 모듈들 조합해서 만든 것이며 (성능 향상에) 핵심적인 부분은 copy mechanism이다. Copy mechanism은 decoder가 단어를 생성할 때 vocab에 등록된 단어 뿐만아니라 input으로 들어오는 source token도 그대로 가져다 쓸 수 있게 하자는 방법이다. 직관적으로 생각해봐도 Input table에 담긴 내용을 natural language로 만들어내야하는 task에 잘 맞는 기법이다. Table에있는 record 하나가 (Point, Russel Westbrook, 50)일 때 우리가 copy해오는 대상은 50, 즉 record의 'value'를 가져오는 것이다.
$$
p(\hat{y}_t | \hat{y}_{1:t-1}, s) = \sum_{z\in\left\{0,1\right\}}p(\hat{y}_t, z_t=z|\hat{y}_{1:t-1}, s)
$$
Copy mechanism을 사용해서 단어 $\hat{y}_t$의 생성확률을 모델링한다는 것은 이 단어가 copy를 통해 만들어졌을 확률과 generation을 통해서 만들어졌을 확률을 구해서 더한다는 의미이다.  확률값 $p(\hat{y}_t, z_t=z|\hat{y}_{1:t-1}, s)$을 구하는 방법으로는 **joint copy model**과 **conditional copy model**이 있다. 둘 다 decoder의 hidden state을 input으로 확률값을 output으로 하는 함수인데, 계산 과정에 차이가 있다. joint model은 $copy(\hat{y}_t)$와 $gen(\hat{y}_t)$를 구한 뒤 normalization term을 이용해 이를 확률값으로 바꾸는 반면, conditional model은 $p_{copy}({\hat{y}_t})$와 $p_{gen}({\hat{y}_t})$이라는 확률값에 $p(z_t=1), p(z_t=0)$이라는 일종의 weight를 이용해서 sum을 한다.

![copy](/assets/img/copy.jpg)

- <u>same-sentence 제한</u>

#### Reconstruction loss

decoder에 hidden state을 이용하여 input의 내용을 맞추는 task를 학습과정에 추가하는 방법이다. 매 time step에서 만들어지는 hidden state가 input에 대한 전역적인 정보를 다 담고 있게끔 도와준다고 이해할 수 있다.

- <u>segment into $\frac{T}{B}$..?</u>

#### TVD Loss

제대로 안 나와 있음

  

## Experiment
![result](/assets/img/wiseman_result.PNG)

- 가장 높은 성능을 낸 건 conditional copy 모델이다.

  

## Issue

- 학습시 truncated BPTT를 사용한다. Summary 평균 길이가 300이 넘는데, 100을 기준으로 truncate를 한다.

- OpenNMT-py 코드를 많이 봐야할듯........ㅠ