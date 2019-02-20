---
layout: post
title: "Python subprocess 모듈"
description: "표준 입출력과 리다이렉션를 이해하고 subprocess 모듈 사용하기"
comments: true
categories: [개발]
tags:
- Python
- Subprocess
- Linux
---



## 표준 스트림

프로세스는 프로그램이 실행 중인 상태를 의미한다. [쉽게 말해 프로그램은 하드에 저장된 코드라면, 이를 실행(메모리에 적재)한 게 프로세스이다.](http://bowbowbow.tistory.com/16) 이러한 프로세스는 표준 입력 스트림(stdin), 표준 출력 스트림(stdout), 표준 에러 출력 스트림(stderr)을 가진다. 각각 프로세스에게 명령(?)하는 경로, 프로세스가 

```python
# test.py
a = input()
with open(a, 'r'):
    print(a.read())
```

'test.py'라는 프로그램이 실행되면 프로세스가 된다. Input()함수는 stdin에서 입력을 받는다. stdin은 **키보드 입력**이다. print() 함수는 stdout로 결과값을 표시해주는 함수인데, stdout은 **모니터 출력**이다. 만약 존재하지 않는 파일 `file.txt`를 열기 시도하면 모니터에 `FileNotFoundError: [Errno 2] No such file or directory: 'file.txt'`라며 오류 메시지가 뜨게 되는데, 이는 stderr을 통해 나온 오류 메시지이다. 오류메시지도 모니터에 출력되기는 하는데, 모니터까지 오는 경로가 stdout이랑은 다른 셈이다.



## 리다이렉션

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



## Subprocess 모듈

[Subprocess 모듈은 파이썬 기본 모듈로, 파이썬 프로그램 내에서 새로운 프로세스를 만들고 여기에 대한 입출력을  관리할 수 있게 해준다.](https://soooprmx.com/archives/5932) 쉽게 말해서 Python 내부에서 다른 프로그램 실행시키는 걸 가능하게 해주는 모듈이다. 예를 들어 파이토치를 이용해서 기계번역 모델을 만든다고 해보자. 열심히 코딩을 해서, 영어 문장을 입력하면 불어문장을 출력하는 거까지 했다고 치자. 이후,성능 측정을 위해서 BLEU score를 재야하는데, 이 코드까지 직접 짤 필요는 없어보인다. 남들도 다 쓰는 성능 지표니까 인터넷에 코드가 널려있으니까. [보통 moses라는 옛날 기계번역 모델때 사용한 multi-bleu.perl 코드를 많이들 가져다 쓴다.](https://github.com/moses-smt/mosesdecoder/blob/master/scripts/generic/multi-bleu.perl) 나는 python으로 코딩을 하고 있지만 subprocess 모듈을 사용하면 내부적으로 perl 코드를 실행시킬 수 있다!

```python
# perl multi-bleu.perl reference.txt < prediction.txt
# shell에서 위 명령어를 실행하는 걸 파이썬 내부에서 하려면 아래와 같이 하면 된다
import subprocess
f = open('prediction.txt', 'r')
subprocess.call(['perl', 'multi-bleu.perl', 'reference.txt'], stdin=f) # 입력 리다이렉션
```

subprocess.call()는 간단한 사용을 위한 함수이다. 보다 세세한 작업을 원하면 subprocess.Popen 클래스의 인스턴스를 만들고 이에 대한 메서드를 사용해야한다. 