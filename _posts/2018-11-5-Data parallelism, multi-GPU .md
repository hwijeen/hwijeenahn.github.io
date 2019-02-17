---
layout: post
title: "Data parallelism, multi-GPU"
description: "Pytorch의 nn.parallel 및 nn.DataParallel 사용하기"
comments: true
categories: [NLP]
tags:
- Pytorch

---

> [파이토치 튜토리얼1](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html), [튜토리얼2](https://pytorch.org/tutorials/beginner/former_torchies/parallelism_tutorial.html), [transformer 코드](http://nlp.seas.harvard.edu/2018/04/03/attention.html#batches-and-masking), [짱짱 블로그](https://medium.com/huggingface/training-larger-batches-practical-tips-on-1-gpu-multi-gpu-distributed-setups-ec88c3e51255?source=user_profile---------2------------------): 짱짱 블로그 좀 더 보고 공부하기



## 개념



## torch.nn.DataParallel

`nn.DataParallel` [implements data parallelism at the module level.](https://pytorch.org/docs/stable/nn.html?highlight=dataparallel#torch.nn.DataParallel) 여기서 말하는 모듈은 `nn.Module` 을 의미한다. Pytorch로 구현하는 딥러닝 모델은 보통 `nn.Module`에서 상속을 받기 때문에, DataParallel을 통해 간단하게 여러 대의 GPU를 사용할 수 있다. 아래 코드 예시에 나와있듯이, 모듈 레벨에서 data parallelism을 하기 위해서는 한 줄 의 코드만 더하면 된다.

```python
import torch.nn as nn

DEVICE = torch.cuda.device('cuda' if torch.cuda.is_available() else 'cpu')

class Model(nn.Model):
    def __init__(self, input_size, output_size):
        super().__init__()
        self.linear = nn.Linear(input_size, output_size) # arbitrary layer
     
    def forward(self, x):
        return self.linear(x)
    
model = Model(50, 30) # instantiate a model as usual
model = nn.DataParallel(model) # as simple as this!
model.to(device=DEVICE)
```

shell에서 `nvidia-smi`를 실행시켜서 GPU사용량을 확인해보면 병렬처리가 잘 작동하는지 확인할 수 있다.





## nn.parallel

`nn.DataParallel`은 내부적으로 nn.parallel 모듈에 정의되어 있는`replicate`,  `scatter`, `parallel_apply`, `parallel_gather`함수를 사용한다. nn.DataParallel을 사용해서 간단히 여러 대의 GPU를 사용할 수 있지만, 여러 부분에서(예를 들어 loss computation 부분) 병렬 처리를 하고 싶으면 이 함수들도 익혀둘 필요가 있다. 각각의 역할을 말로 하면 다음과 같다. 

- `replicate(nn.Module:module, list:devices_ids)` : 인자로 주어진 모듈을 **복사**해서 device_ids로 받은 GPU에 할당한다. 즉, 하나의 모델을 여러 GPU에 올린다. 이 함수는 복사된 모듈이 담긴 리스트를 반환한다.
- `scatter(torch.Tensor:input, list:device_ids)`: 인자로 주어진 텐서를 **쪼개서** 여러 GPU로 보낸다. 텐서를 쪼갠다는 것의 의미는 한 batch내에 있는 여러 개의 example을 device_ids로 주어진 GPU 개수로 나눈다는 것이다. 이 함수는 쪼갠 example들의 tuple을 반환한다.
- `parallel_apply(replicated_modules, scatterd_input)`: 실제로 병렬 연산을 해주는 부분이다. 쪼개진 데이터(subset of batch)는 각각 모델에 들어가서 결과값을 낸다. 반환되는 것은 결과값이 담긴 list다.
- `gather(parallel_apply_output, target_device)`: 병렬 처리된 결과값들을 하나의 텐서로 합쳐주는 부분이다. target_device인자로 넘겨준 GPU에 최종 결과가 안착(?)하게 된다.





## Example

Havard NLP의 transformer 구현중 일부분이다. `nn.DataParallel`은 물론 사용했고, loss 계산 부분까지 병렬처리를 하고자 아래와 같이 코드를 작성했다. 

`self.criterion`으로 들어가는 것은 loss를 계산하는 클래스이다. `nn.NLLLoss`, `nn.KLDivLoss`등은 `nn.Module`을 상속받아 작성된 객체여서 `replicate`의 인자(module)로 들어갈 수 있다!



```python
class MultiGPULossCompute:
    "A multi-gpu loss compute and train function."
    def __init__(self, generator, criterion, devices, opt=None, chunk_size=5):
        # Send out to different gpus.
        self.generator = generator
        self.criterion = nn.parallel.replicate(criterion, 
                                               devices=devices)
        self.opt = opt
        self.devices = devices
        self.chunk_size = chunk_size
        
    def __call__(self, out, targets, normalize):
        total = 0.0
        generator = nn.parallel.replicate(self.generator, 
                                                devices=self.devices)
        out_scatter = nn.parallel.scatter(out, 
                                          target_gpus=self.devices)
        out_grad = [[] for _ in out_scatter]
        targets = nn.parallel.scatter(targets, 
                                      target_gpus=self.devices)

        # Divide generating into chunks.
        chunk_size = self.chunk_size
        for i in range(0, out_scatter[0].size(1), chunk_size):
            # Predict distributions
            out_column = [[Variable(o[:, i:i+chunk_size].data, 
                                    requires_grad=self.opt is not None)] 
                           for o in out_scatter]
            gen = nn.parallel.parallel_apply(generator, out_column)

            # Compute loss. 
            y = [(g.contiguous().view(-1, g.size(-1)), 
                  t[:, i:i+chunk_size].contiguous().view(-1)) 
                 for g, t in zip(gen, targets)]
            loss = nn.parallel.parallel_apply(self.criterion, y)

            # Sum and normalize loss
            l = nn.parallel.gather(loss, 
                                   target_device=self.devices[0])
            l = l.sum()[0] / normalize
            total += l.data[0]

            # Backprop loss to output of transformer
            if self.opt is not None:
                l.backward()
                for j, l in enumerate(loss):
                    out_grad[j].append(out_column[j][0].grad.data.clone())

        # Backprop all loss through transformer.            
        if self.opt is not None:
            out_grad = [Variable(torch.cat(og, dim=1)) for og in out_grad]
            o1 = out
            o2 = nn.parallel.gather(out_grad, 
                                    target_device=self.devices[0])
            o1.backward(gradient=o2)
            self.opt.step()
            self.opt.optimizer.zero_grad()
        return total * normalize
```