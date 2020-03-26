---
layout: post
title: "Network Cheatheet"
description: ""
comments: true
categories: [개발 / 환경]
tags:
- Network
- Cheat sheet
---

## Path from one computer to anoter

[Post about routing](https://run-it.tistory.com/17?category=665122)



## Terms

[IT 동아 computer netwrok의 기초](https://reviewkr.tistory.com/56)

* Broadcast, Subnetmask
* IP: 공인/사설, 고정/유동
* Gateway / DNS
* Protocol(TCP/IP, HTTP, FTP, POP/IMAP/SMTP, DHCP)
* 랜카드와 랜케이블



## Useful commands

```bash
netstat -r # show route table
lsof -Pn -i4 # show open ports 
ifconfig 
traceroute
nmap
host $DOMAIN_NAME # look up domanin name server for ip
scutil --dns # display dns configuration
```

* Network Utilty is an app to do a number of jobs.
* Tech stuff](https://www.zytrax.com/tech/protocols/ip-classes.html)



## SSH

```bash
ssh -J $MIDDLE_USER@$MIDDLE_HOST $REMOTE_USER@$REMOTE_HOST # ssh into remoteuser via middle user
scp -o 'ProxyJump $MIDDLE_USER@$MIDDLE_HOST' $REMOTE_USER@$REMOTE_HOST:$REMOTE_PATH # scp using ssh jump
```



## HTTP

```bash
python -m http.server 60000 --bind 127.0.0.1 # simple python HTTP web server
```



## Connection

```bash
nmap -Pn -p $PORT $REMOTE_IP # port scan. -Pn if host is up for sure
nc -z $REMOTE_IP $PORT # another way of portscan
```



## Python socket

```python
server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1) # to reuse address 
```

* recv, accept are blocking by default!



## Links

https://jvns.ca/blog/2017/09/03/network-interfaces/

https://flaviocopes.com/http-curl/