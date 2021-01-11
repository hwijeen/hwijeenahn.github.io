---
layout: post
title: "GPU and efficiency"
description: "NVIDIA GPU로 빠르게 train, inference 하기"
comments: true
categories: []
tags:
- GPU
- NVIDIA
typora-root-url: ../../hwijeen.github.io
---

# Efficiency

* Train time, Inference time 각각에 적용할 수 있는 테크닉이 다르다.
* Minimize memory access가 핵심! Arithmetic 보다 memory access가 에너지 훨씬 많이 든다.
* Tradeoff between computation and memory bandwidth.
* Nvidia NGC에서 제공하는 컨테이너를 사용하면 dependency에 따른 효율성 문제는 해결된다.

## Training efficiency 

* Parallelization
* Mixed Precision
* Distillation

## Inference efficiency

- Prunning
- Quantization
- Nvidia TensorRT Runtime
- Nvidia Triton Inference Server





### References

[핑퐁 블로그](https://blog.pingpong.us/ml-model-optimize/)

[McKinsey 보고서](https://www.mckinsey.com/~/media/McKinsey/Industries/Semiconductors/Our%20Insights/Artificial%20intelligence%20hardware%20New%20opportunities%20for%20semiconductor%20companies/Artificial-intelligence-hardware.ashx)

[Nvidia 블로그](https://blogs.nvidia.co.kr/2020/02/19/nvidia-tensor-rt/)