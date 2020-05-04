---
layout: post
title: "Python Cheatsheet"
description: "Random things"
comments: true
categories: [Cheatsheet]
tags:
- Python
---



## Colorize output

Terminal에서 출력되는 nerdy하게 output에 색깔을 부여하기 [출처](https://stackoverflow.com/a/287944)

```python
class bcolors:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'
    
print(bcolors.OKBLUE + 'okay this will be blue!')
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



## multiprocessing.pool

많은 양의 데이터를 불러오기, 많은 양의 데이터를 처리해야할 때 용이! 큰 task를 여러 개의 작은 task로 쪼개서 각 process가 하나씩 담당하게 한다.

```python
import multiprocessing as mp

def chunks(l, n):
    chunk_size = len(l) // n
    return [l[i:i + chunk_size] for i in range(0, len(l), chunk_size)]
  
  # Identical result to `result = function(dat_list)`
  num_proc = mp.cpu_count()
  slices = chunks(data_list, num_proc)
  pool = mp.Pool(processes=num_proc)
  result = pool.map(function, slices)
```



## multiprocessing.Process

새로운 프로세스를 생성할 때 stdin은 닫힌다. 새로운 프로세스에서 입력을 받으려면 [임의로 stdin을 열어줘야한다](https://stackoverflow.com/a/30149635)!

```bash

```



