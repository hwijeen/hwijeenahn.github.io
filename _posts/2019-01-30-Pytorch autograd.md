---
layout: post
title: "Pytorch Autograd"
description: "Pytorch autograd, tensor, computation graph등 기본 개념 이해하기"
comments: true
categories: [Python]
tags:
- Autograd
---

Pytorch 기본개념으로써 tensor, computational graph, autograd 등을 잘 설명해놓은 [링크](https://www.kdnuggets.com/2018/04/getting-started-pytorch-understanding-automatic-differentiation.html). 대충 듣던 단어들에 대해서 쉽고 자세하게 얘기해주는 글이다.



## Tensors 

딥러닝이 인기 몰이를 하기 전에는 배열 자료구조를 표현하기 위해 Numpy를 많이 썼다. Numpy는 주요 기능을 C로 구현해놨기에 Python의 list보다 빠르게 가볍다는 장점이 있지만, GPU에 올릴 수 없다는 치명적인 단점이 있다. GPU는 연산을 병렬적으로 처리하는 데 특화되어있는데, 엄청나게 많은 FLOP을 골자로 하는 딥러닝을 위해선 GPU에 올릴 수 있는 배열, Tensor가 필요하다. 

Pytorch tensor class의 대표적인 attribute는 data, grad, grad_fn이다. **data**에는 배열의 원소에 해당하는 값이 저장되어있다. **grad**는 떤 함수를 이 tensor에 대해서 미분한 뒤 evaluate 된 값을 가지고 있다. Backprop을 하기 전까진 None값이 들어있다. **grad_fn**은 gradient computation 과정에서 호출되는 함수로써, "이 텐서가 무슨 operation을 통해 만들었는지"를 나타낸다.

추가로 알아두어야할 attribute: requires_grad, is_leaf



## Computation Graphs

Computation graph는 이름에서 알 수 있듯이 계산 과정을 표현하는 하나의 자료구조이다. 다른 방식으로도 계산 과정을 표현할 수 있겠지만 효율적으로 chain rule을 적용하는데 특화된 건 computation graph이다. 

Pytorch의 computation graph를 그릴 때 각 노드에 실제로 들어가는 것은 tensor의 grad_fn이다. 모든 tensor는 grad_fn을 가지고 있기 때문에 각 노드에 간접적으로 해당하는 tensor가 있다고 생각할 수 있다. 맨날 하는 ```loss.backward()```는 크게 보면 다음과 같이 세 부분으로 구성된다. 

1. 해당 node에서  grad_fn을 이용해 local gradient 계산 

1.  local gradient를 해당 노드(에 해당하는 tensor)의 grad와 곱하기(chain rule) 

1. 계산된 gradient를 input node(에 해당하는 tensor)의 grad에 저장

마지막 node(loss) 부터 맨 앞 node(weight)까지 이 과정을 연쇄적으로 적용하면 gradient of loss w.r.t weight을 얻을 수 있다. 물론 여기서 말하는 gradient는 input을 이용해 evaluated된 값이다. 꼬리에 꼬리를 무는 computational graph가 있기에 '연쇄적'인 chain rule 적용이 가능한 것이다.

Computation graph는 backprop을 하기 위해 필요한 자료구조라고 했다. 그런데 inference시에는 backprop이 필요하지 않다. ```torch.no_grad()```라는 context manager 아래에 inference code를 쓴다는 것은 computation graph자체를 안 만들겠다는 얘기다. 그러면 당연히 time, space가 절약되니까. 이 Context manage 아래서 결과로 나온 tensor는 항상 requires_grad=False이다. 

> Pytorch는 torch.autograd.Function이라는 class로 함수를 관리한다. 이 class는 method로 forward와 backward를 갖는다. 굳이 클래스를 만들어 두 함수를 묶어두는 이유는 backprop과정에서 두 함수가 공유해야할 변수가 있기 때문이다. Pytorch computation graph의 한 node에서 local gradient를 evaluate하기 위해선 backward함수가 forward 함수의 input을 알아야하는 경우가 그렇다.

Pytorch는 dynamic graph 방식, Tensorflow는 static graph 방식이라고들 한다. Dynamic하다는 것은 forward할 때마다 computation 그래프를 새롭게 그린다는 것이다. Model이 있고 거기에 input이 주어질 때마다 forward pass를 할 텐데, 그때마다 다른 computation graph를 그린다. 이런 방식의 장점은 dynamic하게 조작(원한다면 매 forward마다 모델이 다르게 작동하도록 코딩 하기)이 가능하다는 것이다. 디버그도 훨씬 쉽다. Forward때마다 만들어지는 computational graph는 .backward()가 완료될 때 메모리에서 지워진다. Leaf node에 해당하는 weight의 .grad를 계산 및 저장하고 나면 intermediate node의 grad 및 computational graph자체가 지워진다. 그러니까 forward-backward의 반복은 computational graph관점에서는 만들었다 지우기의 반복이다. 

> Torch.autograd.grad함수를 사용하면 ```loss.backward()```가 한정하는 방식 말고도 좀 더 자유로운 미분을 할 수 있다. 예를 들면 함수를 미분하는데 특정 하나의 변수로만 미분하기 같은 계산! 

