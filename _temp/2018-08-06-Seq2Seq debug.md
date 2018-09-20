---
layout: post
title: "Seq2Seq Debug"
description: "Pytorch 구현 때 디버그 체크리스트"
comments: true
categories: [Python]
tags:

---

<br>



##  Special Token

EOS만 붙이기.

SOS는 decoding시 처음에 넣어주는 역할



## Softmax

Softmax할 땐 non-linearity가 없다ㅠㅠ



## Attention

Vectorization!