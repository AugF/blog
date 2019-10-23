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
## 强大的常用命令: find, grep
https://www.cnblogs.com/skynet/archive/2010/12/25/1916873.html

find主要用于查找文件
grep(global search regular expression and print out the line): 强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。

### 1. find
man:
find [-H] [-L] [-P] [-D debugs] [-Olevel] [path] [experssion]

find [path] [experssion]
- path: find命令所查找的目录路径
- experssion: expression可以分为"-options [-print -exec -ok...]"
- -options, 制定find命令下的常用选项
- -print, find命令将匹配的文件输出到标准输出
- -exec, find命令对匹配的文件执行该参数所给出的shell命令

常用选项以及实例
> 可以不用加-print也可以正常输出，那么-print到底有啥用呢
> 在本地测试时发现一个问题，bash test.sh, ok, 但是 ./test.sh permission denied, 不知道是怎么回事
- -name: 按照文件名查找文件
    - find /dir -name filename 在/dir目录及其子目录下面查找名字为filename的文件
    - find . -name "*.c" 在当前目录及其子目录下查找任何扩展名为"c"的文件（即支持正则表达式）
- -perm 按照文件权限来查找文件  
    - find . -perm 755 -print 在当前目录下查找文件权限为755的文件，即文件拥有者可以读、写、执行，其他用户可以读、执行的文件
- -prune 使用这一选项可以使find命令不在当前指定的目录中查找，如果同时使用-depth选项，那么-prune将被find命令忽略
    - find /apps -path "apps/bin" -prune -o -name "a.txt" -print 在/apps目录下查找a.txt，但不希望在/apps/bin目录下查找
        > 对的！不过必须要-print, -o才行，位置也不能错，todo, -o, -print到底是什么作用啊？
- -user: 根据文件拥有者来查找文件
    - find ~ -user sam -print 在$HOME目录中查找文件拥有者为sam的文件
- -mtime -n +n: 根据文件的更改时间来查找文件，-n表示文件更改时间在距现在n天以内，+n 表示文件更改时间距离n天以前
    - find / -mtime -5 -print 在系统根目录下查找更改时间在5日以内的文件
    - find /var/adm -mtime +3 -print 在/var/adm目录下查找更改时间在3日以前的文件
- type: 查找某一类型的文件
    - 类型
        > b块设备文件， d目录， c字符设备文件， p管道文件, l符号链接文件，f普通文件
    - find /etc -type d -print 在/etc目录下查找所有的目录
    - find . !-type d -print 在当前目录下查找除目录以外的所有类型的文件
    - find /etc -type l -print 
- -size n[c]: 查找文件长度为n块的文件，带有c时表示文件长度以字节计，不常用
    - find . -size +10000c -print +表示大于
    - find /home/apache -size 100c print  正好
    - find . -size -10 -print 在当前目录下查找长度不超过10块的文件（一块等于100字节）
- -depth: 在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找
    - find / -name "CON.FILE" -depth -print 它将首先匹配所有的文件然后再进入子目录中查找
- -mount: 查找文件时不跨越文件系统mount点 
    - find . -name "*.XC" -mount -print 从当前目录查找位于本文件系统文件名以XC结尾的文件

### 总结
1. 最常用
    - 查找文件, 可过滤一些文件，？？是否可以用正则表达式
        -name 通配多种文件
    - 时间、目录
        - -mtime 过滤时间
        - -prune 过滤目录
    - 大小、类型
        - -size
        - -type
2. 高级
    - 控制权限
        - -perm 过滤文件自身权限
        - -user 过滤拥有者的权限，
    - 访问顺序
        - -depth
    - 挂载点
        - -mount

### 2. grep
grep [options] pattern [file..]
grep [options] [-e pattern] [-f file] [file ...]
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
## 4. snap ？？？ 什么东西
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

7. mv 使用正则表达式

8. 关于硬盘：rename没有必要

9. linux关于安装系统的命令
    - ln命令： 功能是为某一个文件在另一个位置建议一个同步的链接
        - 例子
            - ln -s link1 link2request to https://registry.npmjs.org/hexo-cli failed, reason: Client network socket disconnected before secure TLS connection was established
                > 为link1文件创建软链接link2, 如果link1丢失，link2将会失效

10. 百度网盘使用有问题，aria2使用频率不高，放弃

11. 安装linux版的hexo
    > 配置文件一般是有全局配置文件的，在～/.npmrc这些内容之下

12. vscode
    > 配置总是让输入user和passwd, git config --global credential.helper store

13. lantern
    > what is proxy IP? 是否会影响其他的安装

14. 在linux上安装pip `sudo apt-get install python-pip(python2)`

