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

