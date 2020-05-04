---
layout: post
title: "Git Cheatsheet"
description: "명령어, 브랜치 관리"
comments: true
categories: [Cheatsheet]
tags:
- Git
- Github/
- Cheat sheet
---

[good blog](https://suwoni-codelab.com/git/2018/04/05/Git-reset/)

## Prep

```bash
git init

git config user.name "hwijeen"
git config user.email "aurorall@naver.com"
- --global 옵션을 사용하지 않은 경우 해당 폴더 scope에서 git 사용자 등록 가능
 
# cf) git clone
git remote add origin $address_copied_from_github_repo
# 주소에 나온 repository를 origin이라는 이름으로 remote에 등록한다
```



## Git의 4개의 영역

![git areas](/Users/hwijeen/Desktop/git areas.png)

#### 1. Working directory

- 코드가 수정되고 저장되는 실제 프로젝트 디렉토리. `.git`을 제외한 모든 영역

#### 2. Index

- Working directory와 repository 사이에 있는 준비 영역.
- Staging area라고도 함.

#### 3. Repository

- 스냅샷을 저장해두는 곳. `.git` 내부에 존재.
- Local repository / remote repository.

#### 4. Stash

- 임시로 작업사항을 저장해놓고 꺼내올 수 있는 별개의 영역.

`git add`로 working directory에서 index로 정보가 저장되고, `git commit`으로 index의 정보를 repository에 저장한다.



## Git file lifecycle

![git file lifecycle에 대한 이미지 검색결과](http://git-scm.com/figures/18333fig0201-tn.png)





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
git commit -a # automatically stage tracked files
git commit --ammend # commit 덮어쓰기

git rm -r --cached $DIRECTORY_NAME$ # remove a folder only from staging area.

git diff # difference between working directory - staging area
git diff --staged # difference between index and repository
# git diff는 
```



#### branch

```shell
git branch # branch list 보여주기
git branch $BRANCH_NAME
git checkout -b $BRANCH_NAME # 만들고 checkout까지
git branch -d $BRANCH_NAME # 삭제
git checkout # 해당 branch로 워킹디렉토리를 이동하기
git branch -m $NEW_NAME # rename
git branch -r # show branches in remote 

git checkout $BRANCH_NAME
git pull origin $BRANCH_NAME
```



#### 병합하기

```bash
git merge $BRANCH_NAME # 지금 branch로 <BRANCH_NAME>을 병합하기 
git rebase -i $BRANCH_NAME # conflict는 손으로 지우고 다시 add해야한다
git checkout $BRANCH_NAME $FILE_NAME # 주의!
```



#### 원격 저장소 관련

```bash
git remote 
git remote update
git remote add $URL

git push origin $BRANCH_NAME # remote repository로 <BRANCH_NAME>을 push

git pull # 현재 branch pull(fetch & commit)
git pull $REMOTE_NAME $BRANCH_NAME # remote의 <BRANCH_NAME>으로부터 pull

git branch -b $BRANCH_NAME $LOCAL_BRANCH_NAME # to get remote branch to current branch
git checkout -t $REMOTE_NAME/$REMOTE_BRANCH_NAME # local에 없는 branch 가져오기

git fetch $REMOTE_NAME

git push origin --delete $REMOTE_BRANCH_NAME # delete branch in remote
git push origin :$REMOTE_BRANCH_NAME # same
```



#### 취소하기

```bash
git reset HEAD $FILE # staging area에서 빼기(add 취소)
git rm --cached $FILE # stage area에서 삭제(파일의 삭제를 add)

git reset --soft HEAD^ # commit 취소하고 working directory, staging area 모두 보존
git reset --mixed HEAD^ # commit 취소하고 working directory 보존, stageing area는 예전 그때로
git reset --hard HEAD^ # commit 취소하고 working directory, staging area 모두 그때로

git reset --hard ORIG_HEAD # pull 취소!
# 이후에 git push -f origin master가 필요
git reset HEAD $FILE # 이 file unstage

git checkout -- $FILE # 최근 commit 상태로 되돌리기, 위험!
```

#### 기타

```bash
git config -l
```



[참고 링크](<https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html>)



## .gitignore

```bash
# comment
*.pyc
data/
__pycache__/
.ideas/
```



## 참고

- [bash prompt에 현재 들어와있는 branch이름 표시하기.](https://coderwall.com/p/fasnya/add-git-branch-name-to-bash-prompt) PS1라는 환경변수가 bash prompt에 나오는 정보를 결정하는 모양인데, 거기에 git branch를 추가해주는 방식인 거 같다. 
- Mac에서 git이 한국어를 하는 경우 [영어로 바꾸기](https://stackoverflow.com/a/54574337)

- [OS간 개행문자 문제](https://www.lesstif.com/pages/viewpage.action?pageId=20776404)

