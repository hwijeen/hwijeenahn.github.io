---
layout: post
title: "Vim Cheatsheet"
description: "Random things"
comments: true
categories: [Cheatsheet]
tags:
- Vim
- Cheat sheet
---



## Misc

```bash
:ls # 현재 위치 확인 가능
:cd # 이걸로 cwd 바꾸기

:%s/$before/$after/g # 파일 전체에서 단어 바꾸기
:s/$before/$after/g # 현재 라인에서만

:set list # show white spaces
:set list! # disable
:set listchars=tab:>>> # display tabs as >>>

:sort u # sort and remove duplicates

:g/$pattern # show lines with pattern
:g/$pattern/d # delete lines with pattern
:g!/$pattern/d # delete lines without pattern

:windo set scrollbind # https://stackoverflow.com/a/2986980
:windo set cursorbind
:syntax sync fromstart " when syntax highlighting is broken
```

시스템 clipboard에 복사하려면 vim 8.1깔고 vim경로 그걸로 잡아주기



## .vimrc

이 파일로 vim 환경 설정을 한다. [vim-bootstrap](https://vim-bootstrap.com/)에서 .vimrc파일을 다운받으면 왠만한 건 다 깔려있다. 하나하나 파악하며 깔다간 정신 건강이 나빠진다.. Plugin manager은 [vim-plug](https://github.com/junegunn/vim-plug)를 쓴다. 아래는 내가 추가한 단축키.

```bash
set mouse=a

"vim-conda
cnoreabbrev conda CondaChangeEnv
let g:conda_startup_msg_suppress = 1

"" Close buffer
noremap <leader>c :bd<CR>

"buffer navigate
nmap <leader>l :bnext<CR>
nmap <leader>q :bp<CR>
nmap <leader>T :enew<CR>

"run python
nmap <F5> :!python %<CR>
```

- Jedi-vim은 안 쓰는 게 나은듯?



## Tab, buffer, window

tab은 작업 단위로 분리해서 쓰고, buffer은 파일을 불러와서 들고있는 공간이고, window는 하나의 버퍼를 보여주는 창이다!

### buffers

```shell
:e #edit a file
:ls # buffer list
:bn # next buffer on window
:bp # previous buffer on window
:badd # add a file to buffer without opening
:bd <BUFFER NAME> or <BUFFER NUMBER> # delete from buffer
:vs | <BUFFER NAME> or <BUFFER NUMBER> # to open a buffer in different window
```

### tabs

```bash
:tabe #edit a file in new tab
:tabclose # close current tab
:tabn # same as gt
:tabp # same as gT
```

[단축키 관련 참고](vim.wikia.com/wiki/Using_tab_pages)



## Macro

```shell
qa # start recording in a
q # finish recording

@a # run macro a
@@ # run last macro run
25@a # run macro a 25 times
```



## Jedi-vim

```bash
<K> # show docstring
<leader>d # go to definition
```



## Misc

[git difftool을 vimdiff로 설정하기](https://stackoverflow.com/a/3713865),  [vimdiff에 대한 간단한 설명]([https://goodtogreate.tistory.com/entry/git-difftool-%EC%82%AC%EC%9A%A9%EB%B2%95](https://goodtogreate.tistory.com/entry/git-difftool-사용법))

```bash
git config --global diff.tool vimdiff
git config --global difftool.prompt false
# git config --global alias.d difftool
```

use `zR` to unfold all. `zM` to fold all.



## Install latest vim

```
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update
sudo apt install vim
```

