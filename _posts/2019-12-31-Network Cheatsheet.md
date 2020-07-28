---
layout: post
title: "Network Cheatsheet"
description: "Random things"
comments: true
categories: [Cheatsheet]
tags:
- Network
- Cheat sheet
---

## Basic Terms

[IT 동아 computer netwrok의 기초](https://reviewkr.tistory.com/56), [Routing](https://run-it.tistory.com/17?category=665122), [Network interface](https://jvns.ca/blog/2017/09/03/network-interfaces/), [curl](https://flaviocopes.com/http-curl/), [ip class](https://www.zytrax.com/tech/protocols/ip-classes.html)



## Useful commands

```bash
netstat -r # show route table
netstat -tnlp # show open ports
lsof -Pn -i4 # show open ports 
ifconfig 
traceroute
nmap
host $DOMAIN_NAME # look up domanin name server for ip
scutil --dns # display dns configuration
```

* Network Utilty is an app to do a number of jobs.



## SSH

```bash
ssh -J $MIDDLE_USER@$MIDDLE_HOST $REMOTE_USER@$REMOTE_HOST # ssh into remoteuser via middle user
scp -o ProxyJump=$MIDDLE_USER@$MIDDLE_HOST $REMOTE_USER@$REMOTE_HOST:$REMOTE_PATH # scp using ssh jump
```



## Basic web server with python

```bash
python -m http.server 60000 --bind 127.0.0.1 # simple python HTTP web server
```



## Connection

```bash
nmap -Pn -p $PORT $REMOTE_IP # port scan. -Pn if host is up for sure
nc -z $REMOTE_IP $PORT # another way of portscan
```





