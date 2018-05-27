---
layout: post
title: "Sublime Text"
description: "Sublime Text 패키지 및 팁들"
categories: [개발 환경]
tags:
- Sublime Text
---



### 기본 사항

1. Sublime Text 설치
2. Package Control 설치
3. ConvertToUTF Package 설치
   - 인코딩 관련 이슈들을 해결해준다.
4. InputArgs 설치

   - build 이후에 argument 받을 수 있도록 해준다

<br>

### Conda 가상환경 사용

1. Sublime Conda Package 설치
   - Preference - Package Setting - Conda - Settings - User파일에서 conda 설치 위치 잡아줄 수 있다.
     - Setting - User 파일은 Packages / User에서 관리되는 듯! User 안에 있는 파일이 우선이고, 그 다음에 Default setting이 적용되는 듯. Default는 안 건드리는 게 현명한 방법.
   - 이후 Command Pallette에서 가상환경 설정 가능

<br>

### 서버 연결

1. ssh 설정하기

   - ssh config 파일 수정하기
   - public key 등록하기

2. Sublime SFTP  설치

   - 서버와 파일 주고 받을 수 있다.
   - 프로젝트 폴더 단위로 Remote Mapping 등록가능.

3. Sublime REPL 설치

   - Sublime Text 안에서 interpreter 사용 가능하게 해준다.

   - Packages - SublimeREPL - config - Python 안에 Default.sublime-commands 와 Main.sublime-menu를 잘 조정해주면 **서버의 interpreter를 킬 수 있다.** 

     - "cmd" 에서 "python"대신 서버의 파이썬 절대 경로 지정해주면 됨.
     - "cmd" 앞에 "ssh" "224" 추가해줘서 일단 서버 접속하도록 한다. 1은 이걸 위한 준비작업.

   - TODO: 서버 인터프리터로 RUN CURRENT FILE하기 

     - 실행하고자하는 파일의 path가 잘 안 잡혀서 오류 난다ㅠㅠ...

     - Sublime Text에서 Jupyter Notebook 5는 안 되는듯. 주피터는 그냥 주피터에서 쓰자.


<br>


###  Tips

- cntl + c : cancel build. Keyboard Interrupt로 사용하면 된다!
- option + down, option + left : Jumb back, Go To Definition