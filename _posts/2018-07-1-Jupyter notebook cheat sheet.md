---
layout: post
title: "서버 이용하기"
description: "리눅스 및 서버에 익숙해지기"
comments: true
categories: [개발]
tags:
- 서버
---



## 주피터 노트북에서 서버 연결하기

[일단 여기를 따라하면 된다](https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh)

그런데 Windows cmd에서 'ssh ~'를 치면 안 된다. openssh라는 패키지가 안 깔려 있기에. 나는 Cygwin이라는 걸 먼저 깔았다. Cygwin은 위도우에서 리눅스 프로그램/유틸리티를 사용 가능하게 해 준다. 이 걸 깔고 가능해지는 것 중 하나가 ls, cd 등 리눅스 기반 명령어를 사용할 수 있는 거. 깔면서 openssh라는 패키지도 같이 깔았다. openssh는 ssh client인듯.

만약 MobaXTerm을 사용한다면 위 과정을 다 스킵해도 된다! MobaXTerm 자체가 Cygwin기반으로 만든 프로그램이고, ssh client기능도 제공한다. 게다가, MobaXTerm을 사용한다면 모니터가 없는 서버에서 jupyter notebook을 실행시켜도 local컴퓨터에서 화면을 볼 수 있게 해준다. 그러니까 위 링크를 따라할 필요가 없다..

MobaXTerm의 모니터 보여주기 기능(?)을 이용하지 않고 Port Forwarding 기능을 이용하고 싶다면 위에 링크 따라하면 된다.

몇 가지 주피터 명령어:

```
# shows list of jupyter notebook servers
jupyter notebook list

# stops localhost:8888
# available on notebook version 5.1.0 or after
jupyter notebook stop 8888
```

jupyter notebook list 상에 많은 서버가 켜져있더라도, 컴퓨터 **재부팅**이후엔 싹 다 꺼진다. 

   

## 주피터 노트북 링크로 접속하기

[링크 1](https://light-tree.tistory.com/111)를 따라하되 **2. 서버 비밀번호 생성**부분은 [링크 2](https://stackoverflow.com/questions/42848130/why-i-cant-access-remote-jupyter-notebook-server) step 4를 따라하고 **3.주피터 서버 환경설정하기**에서 `c=getconfig()`를 지우면 된다



## Display

이 노트북에 한해서 cell width 100%로 늘려주는 [방법](https://stackoverflow.com/a/34058270)

```python
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```





### 

