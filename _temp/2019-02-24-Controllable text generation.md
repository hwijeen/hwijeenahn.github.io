---
layout: post
title: "Controllable text generation"
description: "Conditioned text generation에 관한 논문 리뷰"
comments: true
categories: [논문 리뷰]
tags:
- Text generation
---



Logeswaran et al., 2018. Content preserving text generation with attribute controls.

## Content preserving text generation with attribute controls

### WHAT

주어진 문장에서 내용은 유지하고 attribute만 바꾼 문장을 만들어내는 모델을 제안한다. 기존 VAE 기반 모델은 posterior collapse문제가 있기에 우린 seq2seq 기반 모델을 사용한다. 새로 생성하는 문장은 1)기존 문장의 내용을 담고있어야 한다. 기존에 방법은 auto-encoding기반(copy에 취약) 혹은 back-translation기반(noisy) loss를 통해 기존문장의 내용을 보존하는데, 우리는 둘을 섞은 interpolated reconstruction loss를 사용한다. 2)condition으로 주어진 attribute를 반영하기 위해 adversarial discriminator가 사용되는데, 기존의 방법은 attribute마다 discriminator가 필요하게끔 구성된 반면, 우리는 하나의 discriminator로 여러 attribute에 대처할 수 있는 방식을 제안한다. 

### HOW

