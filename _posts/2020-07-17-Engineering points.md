---
layout: post
title: "Engineering points"
description: "성능을 끌어올리기 위한 디테일들"
comments: true
categories: [Cheatsheet]
tags:
- Engineering
- Cheat sheet
---

## Class imbalance 

* Batch를 구성할 때 class별로 다른 weight를 적용해서 sampling하기. 데이터가 많은 클래스 instance들로만 batch 구성되지 않는 효과를 낸다. Pytorch에서 제공하는 [WeightedRandomSampler](https://pytorch.org/docs/stable/data.html#torch.utils.data.WeightedRandomSampler)을 사용하면 되고, weight을 만드는 방법은 [pytorch discuss](https://discuss.pytorch.org/t/balanced-sampling-between-classes-with-torchvision-dataloader/2703/7)를 참고하기.
* Well classified examples loss에 기여하는 정도가 작아지도록 down weight하는 [focal loss](https://arxiv.org/abs/1708.02002). [Pytorch discuss](https://discuss.pytorch.org/t/focal-loss-for-imbalanced-multi-class-classification-in-pytorch/61289/2)에서 간단한 구현 찾을 수 있음.
* Focal loss의 발전된 형태(인 것같은) [dice loss](https://arxiv.org/abs/1911.02855).



## Random seed

* Random seed에 따라 data order, weight initialization이 달라지고 거기에 따른 성능 차이가 크다는 [논문](https://arxiv.org/pdf/2002.06305.pdf).

* 실험할 때 randomness를 고정하고 싶다면 pytorch, numpy, random 등 사용하는 모든 라이브러리의 seed를 고정해줘야한다.

  ```python
  def set_random_seed(random_seed):
      torch.manual_seed(random_seed)
      torch.backends.cudnn.deterministic = True
      torch.backends.cudnn.benchmark = False
      np.random.seed(random_seed)
      random.seed(random_seed)
  ```



## Multi-GPU training

* 보다 큰 batch-size를 사용하기 위해서, 좀 더 빠른 training을 위해서 필요한 multi-gpu training. [당근 마켓 블로그 글]([https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b](https://medium.com/daangn/pytorch-multi-gpu-학습-제대로-하기-27270617936b))이 좋은 시작점!