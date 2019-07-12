---
layout: post
title: "Probability and Information Theory"
description: "Deep Learning Book 3장"
comments: true
categories: [NLP]
tags:
- Probability
- Information theory
---



## Probability Theory

이공계 전반에서 확률 개념을 사용할 수밖에 없는 이유가 한 문장으로 정리되어있다. 확률을 사용하면 불확실한 진술을 논리적으로 할 수 있다. 머신 러닝을 하다보면 불확실한 값을 다룰 필요가 있는데, 이때 확률 개념이 큰 도움이 된다. 완벽하지만 복잡하고 긴 논리보다는 불확실하더라도 간단한 논리가 필요할 때가 있다. 물론 근거 없는 불확실함은 피해야하고, 확률 이론의 틀 안에서 불확실성을 다뤄야한다. 

- [Random Variable](https://datascienceschool.net/view-notebook/4bcfe70a64de40ec945639236b0e911d/), Probability Distributions,
- Marginal Probability, Joint Probability, marginalization
  - Marginalization은 joint distribution의 단면을 자르는 게 아님에 유의! 
- Conditional Probability, Chain Rule
- Independence and Conditional Independence 

- Expectation, Variance and Covariance
  - Expectation은 함수에 대해 정의된다. [확률 변수도 함수](https://datascienceschool.net/view-notebook/4bcfe70a64de40ec945639236b0e911d/)니까, 확률 변수의 기대값이라는 말도 성립한다.
  - [샘플 분산과 확률 분포의 분산을 구분해서 얘기할 경우](https://datascienceschool.net/view-notebook/e8753e2c0dfd457f91f7b800e340d435/), 샘플 분산은 자료값들과 평균 사이 거리 정도로 해석할 수 있다. 확률 분포의 분산은 확률 변수의 값을 다르게 샘플링함에 따라 그 값들이 얼마나 변하는지를 알려준다.
  - Correlation은 covariance를 [-1,1] 값으로 표준화한 것. 두 값(two values)이 얼마나 선형적으로 연관되어 있는지를 나타내는 척도다. 공분산이 0이라는 것은 두 값 사이에 linear relationship이 없다는 말이다. 두 값이 **독립이란 건 linear 뿐만 아니라 nonlinear relationship도 없다**는 걸 의미한다.

- Bernoulli Distribution, Categorical Distribution
- Gaussian Distribution
- Mixture of Distributions
- Bayes' Rule



## Information Theory

확률 개념을 사용하면 불확실한 진술을 할 수 있다. 정보이론은 불확실한 정도를 정량화해주는 역할을 한다. 예를 들어, 두 확률 분포의 유사성을 수치로 표현할 수 있다. 

- Self-information, Shannon Entropy

- KL-divergence, Cross-Entropy
