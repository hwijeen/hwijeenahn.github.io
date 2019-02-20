---
layout: post
title: "Shell cheat sheet"
description: "개념 및 자주 사용하는 기능들"
comments: true
categories: [개발]
tags:
- Ubuntu
- Bash
- Cheat sheet
---



## 커널 - 쉘 관계



## 몇 가지 문법

```bash
echo $(python -c "print(1)") # $()은 우선적으로 evaluate하라는 의미
ctags -R . $(python -c "import sys; print(sys.path)") # python 실행 결과가 커맨드로 입력됨
echo ${PATH} $ # ${}은 변수 불러오기
```



## Foreground, background

```bash
python a.py & # background 프로세스로 실행
python a.py &> both.txt & # 출력, 오류 스트림 리다이렉션 + background 프로세스로 실행

jobs 
fg %1
bg %1

ctrl + z # stops a process. List of stopped processes in 'jobs'.
ctrl + c to # interrupt(quit)
```

[Background 프로세스는 shell과 독립적으로 실행된다.](https://kb.iu.edu/d/afnz) 즉, 어떤 프로세스를 실행시키고 나서도 터미널 창을 이용할 수 있다! Background 프로세스가 내보내는 stderr, stdout은 방해가 될 때가 많으니까 파일에 적게끔 output redirection을 같이 해주면 편하다.

이미 프로세스를 foreground에서 실행시켜버린 상태라면, ctrl+z로 잠시 stop한 뒤에, jobs에서 bg %1 식으로 background로 보낼 수 있다.