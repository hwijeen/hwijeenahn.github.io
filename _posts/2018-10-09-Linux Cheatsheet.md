---
layout: post
title: "Linux Cheatsheet"
description: "개념 및 자주 사용하는 기능"
comments: true
categories: [Cheatsheet]
tags:
- Ubuntu
- Bash
- Cheat sheet
---

From paste, [awk](http://www.incodom.kr/Linux/기본명령어/awk)

## 커널 - 쉘 - 터미널



## Standard streams

프로세스는 프로그램이 실행 중인 상태를 의미한다. [쉽게 말해 프로그램은 하드에 저장된 코드라면, 이를 실행(메모리에 적재)한 게 프로세스이다.](http://bowbowbow.tistory.com/16) 이러한 프로세스는 표준 입력 스트림(stdin), 표준 출력 스트림(stdout), 표준 에러 출력 스트림(stderr)을 가진다.

```python
# test.py
a = input()
with open(a, 'r'):
    print(a.read())
```

'test.py'라는 프로그램이 실행되면 프로세스가 된다. Input()함수는 stdin에서 입력을 받는다. stdin은 **키보드 입력**이다. print() 함수는 stdout로 결과값을 표시해주는 함수인데, stdout은 **모니터 출력**이다. 만약 존재하지 않는 파일 `file.txt`를 열기 시도하면 모니터에 `FileNotFoundError: [Errno 2] No such file or directory: 'file.txt'`라며 오류 메시지가 뜨게 되는데, 이는 stderr을 통해 나온 오류 메시지이다. 오류메시지도 모니터에 출력되기는 하는데, 모니터까지 오는 경로가 stdout이랑은 다른 셈이다.



## Redirection

[리다이렉션은 표준 스트림의 흐름을 바꿔주는 것이다.]((https://jdm.kr/blog/74)) `test.py`는 stdout(모니터 화면)에 파일 `a`의 내용을 뿌리게끔 되어있는데, `a`의 내용을 `output.txt`라는 파일에 저장하고 싶다면 다음과 같이 하면 된다. 화면에 뭐가 너무 많이 나와서 보기 싫거나, 화면에 나온 결과를 저장하고 싶을 때 쓸 수 있다.

```bash
python test.py > output.txt
# 물론, 실행 이후에 a에 들어갈 파일 이름은 키보드로 입력해주어야한다
```

위 명령어를 실행하면 모니터엔 아무런 내용이 뜨지 않고, 모니터에 떴어야할 내용이 output.txt에 쓰이게 된다. 즉, 프로세스의 출력 스트림을 파일로 리다이렉션 한 것이다.

출력 스트림이 아니라 입력스트림을 리다이렉션할 수도 있다. `a`라는 변수에 들어갈 파일 이름을 키보드로 직접 입력하지 않고, 다른 파일에서 읽어올 수 있다는 말이다. 아래의 명령어를 실행하면 `input_file.txt`의 내용이 모니터에 출력되게 된다.

```bash
python test.py < filename.txt
# filename.txt 내용
input_file.txt
# input_file.txt 내용
hi my name is hwijeen
```

입력과 출력에 대해 모두 리다이렉션을 할 수도 있고, 오류 스트림에 대한 리다이렉션을 할 수도 있다.

```bash
# 입출력 모두 리다이렉션
python test.py < filename.txt > output.txt
# 오류 스트림 리다이렉션. 오류 발생시 오류 메시지가 파일에 저장된다.
python test.py < filename.txt 2> output.txt
# 출력, 오류 스트림 모두 리다이렉션
python test.py &> output.txt
```



## Environmental variables

```bash
python example.py
```

shell에서 위와 같이 입력하면 example.py가 실행되는데, 이 과정에서 사실 환경 변수가 사용된다. example.py라는 파일을 python으로 실행하라고 명령한 건데, 컴퓨터 내에 python이 어디에 위치했는지 알려주는 게 환경변수의 역할. ([개념 좀 더](정확한 개념은 여기서](http://blog.naver.com/PostView.nhn?blogId=koromoon&logNo=220793570727)))

여러 환경 변수 중에 'PATH'라는 환경변수는 실행파일을 찾는 경로를 알려준다. 이 변수에 python 위치, cudnn 위치 등이 잡혀있어야 된다. 예를 들어, Python위치가 제대로 잡혀있지 않으면 *python: command not found* 오류가 난다.

Unix 계열 운영체제에서 환경 변수는 '~/.bash_profile', '~/.bashrc'에 저장해둔다. 터미널을 실행할 때마다 운영체제가 이 파일을 호출해서 환경 변수 값을 읽는 구조다. MacOS를 사용한다면 '~/.bash_profile', Ubuntu라면 '~/.bashrc'에 환경 변수 등록을 추천한다([자세한 차이는 여기](http://uroa.tistory.com/114)). 파일 하단에 다음과 같은 export 명령어를 추가해서 환경 변수 등록을 할 수 있다. **주의할 점은 '=' 좌우로 띄어쓰기를 하면 안 된다는 점이랑 ':'을 통해서 여러 경로를 등록한다는 거!**

```bash
# '/'는 최상위 directory를 의미
export PATH="/Users/hwii/anaconda/bin:$PATH"
```

Finder를 통해서 위 경로를 따라가 보면 'python3.6'과 같은 실행파일이 있는 걸 확인할 수 있다.

'~/.bash_profile', './bashrc'파일을 수정한 뒤엔 다음과 같은 확인 작업을 거친다.

```bash
# 변경사항 적용
source ~/.bash_profile
# PATH 환경변수에 등록된 값 확인
echo $PATH
```

참고로, 가상 환경 사용시 가상 환경 내부에 있는 python 경로를 직접 등록해줄 필요는 없다. 'source activate 가상환경'을 할 경우 anaconda와 같은 가상환경 매니저가 알아서 가상환경 내부 python 경로를 path에 등록해준다.

```bash
source activate hwijeen_2.7
# PATH 확인시 내가 직접 등록하지 않은 python_2.7도 잡혀있음을 볼 수 있다. anaconda가 대신 잡아준 거다.
echo $PATH
# 모든 환경변수 보기
env
```



## Tar

```bash
tar -cvzf xxx.tar.gz * # 해당 경로의 모든 파일을 압축
tar -xvzf xxx.tar.gz
```



## Port forwarding

```bash
ssh -N -f -L 8888:localhost:9999 remote_user@remote_server
# -N no remote command
# -f put ssh in the background
# -L forward A:B
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



## Package management

```bash
sudo apt-get update # update package index
apt list --installed	# show list of installed packages
sudo apt-get install $PACKAGE_NAME
sudo apt-get remove $PACKAGE_NAME
sudo apt-cache search $KEYWORD
```



## 초기설정

```bash
# Add user
sudo adduser $USER_NAME
sudo usermod -aG sudo $USER_NAME # add to sudo group

# Install anaconda
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh

# Install zsh
zsh --version # check if already installed
sudo apt-get install zsh
chsh -s /usr/bin/zsh

# Install Oh My Zsh
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh 

# dotfiles
.zshrc
.vimrc
```



## Google drive link wget으로 다운로드

[출처](https://medium.com/@acpanjan/download-google-drive-files-using-wget-3c2c025a8b99)

```bash
#https://drive.google.com/file/d/1UibyVC_C2hoT_XEw15gPEwPW4yFyJFeOEA/view?usp=sharing
# FIELDID=1UibyVC_C2hoT_XEw15gPEwPW4yFyJFeOEA
# FILENAME=file.txt

wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=FILEID' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=FILEID" -O FILENAME && rm -rf /tmp/cookies.txt
```

