---
layout: post
title: "CUDA, Cudnn"
description: "딥러닝에 필요한 cuda, cudnn 설치 및 환경 변수 설정"
comments: true
categories: [개발 / 환경]
tags:
- 환경변수
- Cudnn
- CUDA
---

## CUDA, Cudnn

CUDA: GPU를 그래픽 계산 뿐만 아니라 사용자에 목적에 맞게끔 사용할 수 있도록 (GPGPU) Nvidia가 만들어 놓은 라이브러리다. 라이브러리는 추상적인 개념이지만 CUDA는 결국엔 컴퓨터에 .so 확장자를 갖는 파일로 존재한다. Pytorch 등 프레임워크는 내부적으로 CUDA를 사용하는데, CUDA에 접근하려면 바이너리 파일이 어디에 있는지 알 필요가 있다. 이 위치를 알려주는 게 LD_LIBRARY_PATH다. 이 변수에 CUDA위치가 잘 잡혀있다면 Pytorch가 오류 없이 돌아갈 것이다! **Pytorch 혹은 Tensorflow 버전마다 지원하는 CUDA 버전이 다르므로, 지원 버전을 잘 확인하고 CUDA를 버전을 선택해서 다운 받아야한다.** [여러 버전의 CUDA를 깔아두고, 가상환경 별로 다른 CUDA를 잡아줄 수도 있다](https://blog.kovalevskyi.com/multiple-version-of-cuda-libraries-on-the-same-machine-b9502d50ae77). Pytorch는 사용자가 직접 CUDA 경로를 건드리지 않아도 되게끔 자체적으로 CUDA를 관리한다(따로 지정된 곳에 설치).

Cudnn:  [딥러닝에 GPU를 활용할 수 있도록 해주는 라이브러리로, CUDA를 사용해 만들어져있다.](https://www.quora.com/What-is-CUDA-and-cuDNN) CUDA위에 있는 라이브러리다. 그러니까, 파이토치는 Cudnn을 사용하고, Cudnn은 CUDA를 사용하는 구조다.

**CUDA 및 Cudnn는 설치 이후에 직접 환경 변수 설정까지 해줘야한다**. 설정 과정 설명은 구글링하면 친전한 글들이 많이 나온다.만약에 환경 변수 설정을 안 할 경우 혹은 잘못한 경우 다음과 같은 에러가 난다. 

```bash
ImportError: libcudart.so.8.0: cannot open shared object file: No such file or directory
```

Pytorch가 CUDA 위치를 잡지 못해서 나는 에러다. CUDA 위치를 LD_LIBRARY_PATH 환경 변수에 등록해주면(위에서 설명한 방법대로) 문제 해결 가능하다.  

```bash
# '~/.bashrc'
# 기본적으로 아래와 같은 위치에 설치된다. 8.0 버전임에 유의!
LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH # 꼭 기존 거 앞에!
```

이때, CUDA 파일이 다른 가상환경 안에 있을 수도, usr/local/에 있을 수도 있다! CUDA를 설치했는데 위치를 모르겠다면 터미널에서 다음 명령어로 파악이 가능하다.

```bash
# CUDA 파일 중에 하나. 이 파일이 들어 있는 폴더가 CUDA가 설치된 경로다.
# MacOS에선 locate 명령어 안 됨
locate libcudart.so.8.0
```

* [NVIDIA RTX는 CUDA 9.0에서 오류가 난다.](https://github.com/pytorch/pytorch/issues/17543) CUDA 10.0을 쓰려면 Pytorch 1.0이 필요하다.
* [Cudnn 설치는  CUDA 폴더에 파일 몇 개 넣기](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html). Cudnn버전만 바꿀 때 참고할 [링크](https://stackoverflow.com/questions/38137828/how-do-i-update-cudnn-to-a-newer-version)

```python
torch.version.cuda # returns version of the cuda found by pytorch
```





## 기타 지식

.a파일은 정적 라이브러리고 .so파일은 동적 라이브러리다. 

ls -la를 통해 symbolic link 확인할 수 있다.

Anaconda는 가상환경마다 환경 변수를 따로 관리한다. $CONDA_PREFIX는 지금 activate된 가상환경 위치 알려주는 환경 변수.

기본적으로 /usr/local/cuda-9.0에 쿠다가 깔려있다. 가상환경을 만들고 CUDA를 별도로 설치했을 경우, $CONDA_PREFIX/lib 안에도 CUDA 바이너리 파일이 존재한다.

