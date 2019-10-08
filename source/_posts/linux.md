---
title: linux
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:28:11
tags:
- linux
- snap
- lua
- todo
- chrome
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

## apt install 
https://www.cnblogs.com/hanxing/p/3996103.html

### 1. 安装位置
/var/cache/apt/archieve 软件的安装缓存
sudo apt-get autoclean 只删除低版本的deb包
sudo apt-get clean 全部删除
> 为了以后安装系统方便，可以将这些deb包保存在其他地方

一般的deb包都安装在/usr 或 /usr/share /usr/local中。
自己下载的压缩包或编译的包，有些可以选择安装目录， 一般放在/usr/local. 有时也放在/opt中。

如果想知道具体位置， dpkg -L XXX.deb
> eg: dpkg -L firefox

如果想知道apt-get install 安装的软件
dpkg -S softwarenmae | grep cnf$

1. dpkg -L  
2. dpkg -S  apt-get install
    > whereis 查看命令位置
3. /usr,  /usr/share,  /usr/local, /opt

!!!! usr/share correct!!!

### 2. apt-get

apt-get: advanced packaging tools
> apt-get需要root,  apt只需要当前用户

`apt-get <command> [<option>] pkg1 [pkg2..]`
1. command:
    - update 重新获取软件包列表，很多时候软件安装不上就要先进行update一下
    - upgrade 进行更新
    - install, remove
    - autoremove: 自动移除全部不使用的软件包
    - purget 移除软件包和配置文件诶见
    - source 下载源码档案
    - autoclean
2. args
    - -h 帮助文件
    - -q 输出到日志 无进展指示
    - -qq 不输出信息，错误除外
    - -d 仅下载， 不安装或解压归档文件
3. 常用实例
    - apt-cache search pkg

## 3. add a path to PATH

~/bin: some distributions automatically put in your PATH if it exists

1. add a path to PATH:  `PATH=$PATH:~/opt/bin`
    - way1: 需要重启
        - ~/.profile 当前用户
            > profile 侧面，轮廓
        - /etc/profile 所有用户
    - way2: ~/.bashrc
        - vim ~/.bashrc
        - source ~/.bashrc
2. check the path
    - echo $PATH 检查环境变量
        > echo 重复，反射； 回应
## 4. snap 
add /snap/bin to $PATH

then can directly use


## 5. lua环境安装
1. sudo apt-get install lua5.3
2. sudo apt-get install luarocks
    > luarocks install gnuplot: install torch first
3. git clone torch
    > bash install-deps
    > lua5.2  torch7
    > th 快捷命令
## todo

1. proxy problem: 开机手动设置
2. 查看后台进程
    - ps
    - service
        > chkconfig --list
        > no find!!!

3. uname -a: 查看当前版本信息

4. chrome 快速定位到搜索栏: ctrl+L, alt+D

5. 最小化所有窗口 win + d ？ 最小化当前窗口， 终端重启一个

6. to add deepin some memory