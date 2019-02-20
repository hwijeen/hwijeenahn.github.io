---
layout: post
title: "Anaconda cheat sheet"
comments: true
description: "자주 쓰는 기능들 정리"
categories: [개발]
tags:
- Anaconda
- 패키지 관리
- 가상환경
- Cheat sheet
---



## 개념

Anaconda는  **패키지 관리** & **가상환경 관리**용 프로그램이다. 	



## 패키지 관련 명령어
```
conda install $PACKAGE_NAME
conda install --name $ENVIRONMENT_NAME $PACKAGE_NAME
conda update --name $ENVIRONMENT_NAME $PACKAGE_NAME
conda update conda
conda remove --name $ENVIRONMENT_NAME $PACKAGE_NAME
conda list --name $ENVIORNMENT_NAME 
```



## 가상환경 관련 명령어

```
conda create --name $ENVIRONMENT_NAME python=3.6 $PACKAGE_NAME
source activate $ENVIRONMENT_NAME 
source deactivate
conda env remove --name $ENVIRONMENT_NAME
conda info --envs
conda env export -n $ENVIORMENT_NAME > environment.yml
conda env create -f enviornmnet.yml
```



## conda install vs. pip install

conda install은 conda repository, pip install은 PyPI에 있는 패키지 파일을 설치한다. 같은 내용의 패키지 일지라도 conda repository는 conda package형식으로 패키지를 가지고 있고, PyPI는 wheel 파일 혹은 source 파일 형태로 패키지를 저장하고 있다. 

conda를 이용해서 가상환경을 만들시 환경 내에 기본적으로 pip도 같이 설치된다. 간혹 conda install $PACKAGE_NAME을 했는데 패키지를 찾을 수 없다고 나오는 경우(conda repository에 없어서) pip install을 통해 패키지 설치를 하면 된다. 주의할 점은 **가상환경 내의 pip**를 써야한다는 것. 커맨드 라인에 pip install을 치면 global version이 실행된다. [가상환경 내의 pip 경로를 직접 잡아서 실행시켜줘야한다](https://www.puzzlr.org/install-packages-pip-conda-environment/). 



