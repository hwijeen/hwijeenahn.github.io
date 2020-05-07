---
layout: post
title: "Shell Cheatsheet"
description: "간단한 데이터 처리, 실험 관리용"
comments: true
categories: [Cheatsheet]
tags:
- Shell script
---

From [cheatsheet](https://devhints.io/bash), [for loop](https://www.cyberciti.biz/faq/bash-for-loop/).

## Variables

```bash
A="Hi"
echo $A

a=$(pwd) # command substitution
echo $(python -c "print(1)") # $()은 우선적으로 evaluate하라는 의미(shell execution)
ctags -R . $(python -c "import sys; print(sys.path)") # python 실행 결과가 커맨드로 입력됨
```



## For loops





## Some bash commands

```bash
find . -name '*keyword*'
find . -name '.*' -delete # recursively delete all the hidden files
grep -r -n -i --exclude-dir=data/ 'keyword' . # -i 대소문자 구별 안 함 -n line number

scp -r $localfolder hwijeen@163.239.199.230:$remotefolder/ # from to

cd folder && ls # second commnad executed only if the first succeeds
cd folder; ls # just two independent commands

sort -R input | head -n 100 >output # randomly select sentences

date; python program.py; date # to check runtime

tail -n +2 $filename # from 2nd line to last(skip header)

awk '{T+= NF} END { print T/NR}' $FILE # average length of tokens 
awk '{print NF}' | sort -n | tail # lengths of longest lines

cut -d '.' -f3- $filename # cut 3rd fields to the end
sed -e 's/^"//' -e 's/"$//' $filename # delete heading and trailing "
```

