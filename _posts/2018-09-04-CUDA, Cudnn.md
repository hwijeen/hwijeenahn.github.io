---
layout: post
title: "CUDA, Cudnn"
description: "Pytorch 설치시 CUDA, Cudnn issue 관련사항"
comments: true
categories: [개발]
tags:
- Pytorch
- Linux

---



## 개념

CUDA는 ~~을 하는 라이브러리다. 머신에 .so 확장자를 갖는 바이너리 파일로 존재한다. Pytorch 등 프레임워크가 CUDA에 접근하려면 바이너리 파일이 어디에 있는지 알 필요가 있다. LD_LIBRARY_PATH에 CUDA위치가 잡혀있다면 Pytorch가 잘 돌아갈 것이다! 여러 버전의 CUDA를 깔아두고, 가상환경 별로 다른 CUDA를 잡아줄 수도 있다 [참고](https://blog.kovalevskyi.com/multiple-version-of-cuda-libraries-on-the-same-machine-b9502d50ae77)

Cudnn:



## 에러 및 대처

```bash
ImportError: libcudart.so.8.0: cannot open shared object file: No such file or directory
```

python 스크립트를 실행했는데 위와 같은 에러가 났다. Pytorch가 CUDA 위치를 잡지 못해서 나는 에러다. CUDA 위치를 LD_LIBRARY_PATH에 등록해주면(export) 문제 해결 가능하다.

이때, CUDA 파일이 다른 가상환경에 있을 수도, usr/local/에 있을 수도 있다!



## 기타 지식

.a파일은 정적 라이브러리고 .so파일은 동적 라이브러리다. 

ls -la를 통해 symbolic link 확인하기.

가상환경마다 환경 변수 다르게 관리한다. 가상환경에 환경 변수 추가하는 방법. $CONDA_PREFIX 환경 변수.

locate libcudart.so.8.0 명령어로 CUDA 바이너리 파일이 어디에 설치되어있는지 확인할 수 있다.

기본적으로 /usr/local/cuda-9.0에 쿠다가 깔려있다. 가상환경을 만들고 CUDA를 별도로 설치했을 경우, $CONDA_PREFIX/lib 안에도 CUDA 바이너리 파일이 존재한다.