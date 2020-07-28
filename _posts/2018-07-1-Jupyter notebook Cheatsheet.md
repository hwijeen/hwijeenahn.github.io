---
layout: post
title: "Jupyter Notebook Cheatsheet"
description: "서버에서 사용, 커스터마이징"
comments: true
categories: [Cheatsheet]
tags:
- 서버
- Jupyter notebook
---

## 명령어

```bash
# shows list of jupyter notebook servers
jupyter notebook list

# stops localhost:8888
# available on notebook version 5.1.0 or after
jupyter notebook stop 8888
```

jupyter notebook list 상에 많은 서버가 켜져있더라도, 컴퓨터 **재부팅**이후엔 싹 다 꺼진다. 



## Port forwarding 활용해 로컬에서 원격 주피터 노트북 접속

[출처](https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh)

```bash
# on remote host
jupyter notebook --no-browser --port=8889
# on local machine
ssh -N -f -L 8888:localhost:8888 $REMOTE_USER@$REMOTE_HOST
# -N for no remote command to be executed
# -f for ssh in background
# -L port forwarding
```



## 원격 주피터 노트북 서버에 접속하기

[링크 1](https://light-tree.tistory.com/111)를 따라하되 **서버 비밀번호 생성**부분은 [링크 2](https://stackoverflow.com/questions/42848130/why-i-cant-access-remote-jupyter-notebook-server) step 4를 따라한다. 만약 안 된다면 유력한 이유는 port 접근 자체가 막힌 거..

```bash
sudo ufw allow 8888
jupyter notebook --generate-config
```

```bash
jupyter notebook password
```

```bash
#	in ~/.jupyter/jupyter_notebook_cofing.py
c.NotebookApp.allow_origin = '*'
c.NotebookApp.ip = '$IP'
c.NotebookApp.notebook_dir = '$DESIRED_PATH'
c.NotebookApp.password = $COPIED_RESULT
c.NotebookApp.open_browser = False
c.NotebookApp.port = '8888' # 8888 is default
```

```bash
# on local browser
$IP:8888 
```



## Misc

### Cell width

결과 창이 가로로 너무 작아서 잘릴 때 대처방법 [출처](https://stackoverflow.com/a/34058270)

```python
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```



### Background

```bash
jupyter notebook > /dev/null 2>&1 &
# redirection first, and then to background
```



### Jupyter themes

주피터 노트북 테마 사용법 [출처](https://pinkwink.kr/1120)

```bash
pip install jupyterthemes
jt -t grade3 -fs 12 -tfs 12 -nfs 11 -ofs 12 -cellw 980 -T -f hack -N
```



## vim-binding

[nbextension](https://github.com/lambdalisue/jupyter-vim-binding)

