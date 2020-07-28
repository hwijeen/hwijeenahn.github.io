---
layout: post
title: "AWS Cheatsheet"
description: "Random things"
comments: true
categories: [Cheatsheet]
tags:
- AWS
- Cheat sheet
---

## aws-cli

우선 cli 환경에서 aws 이용하기 위해 [프로그램 설치](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-linux.html)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```



## aws-configure

IAM console에서 볼 수 있는 액세스 키, 비밀 액세스 키를 등록해주기 [링크1](https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/id_credentials_access-keys.html), [링크2](https://aws.amazon.com/ko/blogs/security/wheres-my-secret-access-key/)



## file download

[aws에 공개된 public dataset 다운받기](https://stackoverflow.com/questions/61808322/how-can-i-download-an-aws-open-data-set-to-my-machine)

```bash
# arn:aws:s3:::amazon-reviews-ml
aws s3 ls s3://amazon-reivews-ml # to ls
aws s3 cp s3://amazon-revies-ml . --recursive  # to download to local

```



