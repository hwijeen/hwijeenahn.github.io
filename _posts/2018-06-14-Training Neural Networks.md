---
layout: post
title: "Training Neural Networks"
description: "Practical Tips"
comments: true
categories: [NLP]
tags:
- Machine Learning Basics

---

From CS231n lecture note & assignments, Neural Network Design, Cho's lecture



# Preprocessing

1. Data Split





# Training

<u>Hypothesis Set을 설정하고, 그 안에서 최적의 model을 찾는 과정</u>. Hyperparameter을 조정하는 것도 하나의 hypothesis set을 만드는 것과 동일하다. Training을 통해 최적의 model, 즉 최적의 parameter을 찾는 과정이 필요하다.

1. 모델 architecture
   - Problem specific. Always consider *information flow!*
   - A bunch of **Hyperparameters** associated with the model.
2. Weight Initialization
   - Small random numbers from Gaussian or Uniform distribution.
   - Consider using **Batch Normalization**(or its variation) for more robust weight initialization.
3. Rugularization
   - **Dropout** or its variants. 
   - **Gradient Clipping**. 
4.  Loss function
   - If the output of a network is a distribution(multinomial, via Softmax function[^1]) natural choice is to use **Negative Loss Likelihood**.
5.  Optimization algorithm
   - Adaptive Learning rate algorithm is typically preferred(**Adam**, etc).
   - Use **Stochastic** update rule[^2], using mini-batch.
6. Stopping Criteria
   - Early stopping with **Validation Set** performance. Validation performance is occasionally measured.
7. Ensemble
   - Performance boost with more computational cost.



# Visualization

1. Attention











[^1]: 네트워크가 distribution을 뱉어내게 하면서, $$ f_\theta(x) = ?$$을 찾는 머신러닝 문제가 $$ P(y=y' | x) = ?$$를 찾는 문제로 전환된다. 즉 함수를 찾는 문제에서 확률밀도함수를 찾는 문제로 바꾸는 것! 이렇게 되면 확률에 관한 각종 도구를 쓸 수 있다는 장점이 있다. 
[^2]: mini-batch의 stochasticity 덕에 regularization이라는 부수적인 효과를 얻을 수도 있다.





----

### TODO:

- 분류 문제에서 MSE 못 쓰는 이유?
- 다항분포에서  likelihood 전개하기
- Validation measure하는 주기 