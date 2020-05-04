---
layout: post
title: "HomeBrew Cheatsheet"
description: ""
comments: true
categories: [Cheatsheet]
tags:
- Docker
- Cheat sheet
---

## Brew

각종 프로그램 설치를 도와주는데, 깔끔한 환경 관리를 위해 되도록이면 brew를 쓰자. 설치한 프로그램은 `usr/local/Cellar`에 저장되며, `/usr/local/bin`에 symbolic link가 생성된다.

```bash
brew list 

brew install tldr
brew install mysql
brew install vim # for +clipboard enabled vim
brew install gcc@8
```



## Brew Cask

Mac app을 CLI 환경에서 간단하게 깔아주는 프로그램

```bash
brew cask list

brew cask install java
```



## Brew service

```bash
brew services start $PROGRAM # start at login
brew services restart $PROGRAM # it's like restarting..

brew services run $PROGRAM # run but don't start it at login

brew servies list # list all services managed by brew servies

brew services cleanup # remove all unused servies
```



## Brew tap

`brew tap` adds more repositories to the list of formulae that `brew` tracks, updates, and installs from. By default, `tap` assumes that the repositories come from GitHub, but the command isn’t limited to any one location.



## Dependencies

https://blog.jpalardy.com/posts/untangling-your-homebrew-dependencies/