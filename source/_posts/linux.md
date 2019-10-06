---
title: linux
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:28:11
tags:
- linux
categories:
- tools
- linux
---
## 作业相关

jobs 查看正在进行的job
ps 查看正在进行的进程

jobs 挂起
kill -stop PID
kill %job_num
kill pid
> 先经过ps查看一下

## 查看端口
1. linux
netstat -lnp | grep 8080
ps -aef | grep tomcat

2. windows

netstat -ano | findstr 8080
taskkill /F /pid 1088

## ssh使用

1. 查看ip地址， ifconfig
2. sudo apt install openssh-server
