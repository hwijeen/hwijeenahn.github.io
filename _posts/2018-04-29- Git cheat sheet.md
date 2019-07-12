---
layout: post
title: "Git cheat sheet"
description: "헷갈리는 git 사용법, 몇 가지 git 문제상황 및 해결법"
comments: true
categories: [개발]
tags:
- Git
- Github
- Cheat sheet
---



## Git

로컬 폴더에서 git & github을 사용하려면 절차!

서버에서 git을 쓰려니 다른 사용자에게도 영향을 미칠까봐(?) 무서웠는데 폴더 별로 사용자 지정이 가능하다.

```
1. git init

2. git config user.name "hwijeen"
   git config user.email "aurorall@naver.com"
   - --global 옵션을 사용하지 않은 경우 해당 폴더 scope에서 git 사용자 등록 가능
 
# cf) git clone
3. git remote add origin $address_copied_from_github_repo
   - 주소에 나온 repository를 origin이라는 이름으로 remote에 등록한다
   - git remote시 remote 저장소 list 볼 수 있다
   - http:// 주소를 복사해올 경우 push할 때마다 github ID/password 입력해줘야한다
   
4. git add / commit / push 이제 사용 가능!
```



## 브랜치 관리 정책

1. master branch: 

   - 완전한 코드
   - 버전 단위로 관리한다

2. dev branch:

   - stable하지만 개발중인 브랜치

   - 협업할 때 각자의 브랜치에서 작업한 내용 local의 dev로 merge한 뒤, 그 브랜치를 github의 dev로 push한다. 
   - 이 브랜치의 내용 자주 확인할 필요 있다. merge & pull을 통해

3. 임의의 작업 branch

   - 내가 지정한 단위의 작업을 하는 브랜치
   - 여기서 commit은 내 맘대로
   - 지정한 단위의 작업이 다 끝나고 test까지 완료되면 local의 dev branch로 merge



## 명령어

#### 기본 명령어

```bash
git add . --all # --all 옵션으로 '지운 파일'까지 track, stage
git commit -m "commit message"
git push origin <BRANCH_NAME>

git rm -r --cached <DIRECTORY_NAME> # remove a folder only from remote area, not local device
```

#### branch 만들기

```shell
git branch # branch list 보여주기
git branch -n <BRANCH_NAME> # 만들고 checkout까지
git branch -d <BRANCH_NAME> # 삭제
git checkout # 해당 branch로 이동하기
```

#### 병합하기

```bash
git merge <BRANCH_NAME> # Head로 <BRANCH_NAME>을 병합하기 
git rebase? # conflict는 손으로 지우고 다시 add해야한다
```

#### 원격 저장소 관련

```bash
git remote 
git remote add <URL>
git push origin master # origin이라는 원격 저장소로 master이라는 로컬 저장소의 내용을 push한다
git pull

git fetch?
```

#### 기타

```bash
git config -l
```

#### 취소하기

```bash
git reset HEAD <file> # staging area에서 빼기(add 취소)

git reset --soft HEAD^ # commit 취소하고 해당 파일들은 stage상태로 워킹 디렉토리에 보존
git reset --mixed HEAD^ # commit 취소하고 해당 파일들은 unstage상태로 워킹 디렉토리에 보존
git reset --hard HEAD^ # commit 취소하고 해당 파일들은 워킹 디렉토리에서 삭제!
```

[참고 링크](<https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html>)



## .gitignore

```bash
# comment
*.pyc
data/
__pycache__/
```



## BTS 혹은 Trello

dev에서 branch를 따와서 그곳에서 작업한다. 그 branch에서 하는 작업에 대한 상세한 설명 써놓고 관리. 커뮤니케이션은 여기서!



## 참고

[bash prompt에 현재 들어와있는 branch이름 표시하기.](https://coderwall.com/p/fasnya/add-git-branch-name-to-bash-prompt) PS1라는 환경변수가 bash prompt에 나오는 정보를 결정하는 모양인데, 거기에 git branch를 추가해주는 방식인 거 같다. 



