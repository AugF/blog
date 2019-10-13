---
title: git using
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-18 14:24:20
tags:
- tools
- git
categories:
- [tools, git]
---

更全面的请参考
http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html

## 绪论
Workspace：工作区
Index / Stage：暂存区  add
Repository：仓库区（或本地仓库） commit/fetch
Remote：远程仓库

git reflog 查看当前分支的最近几次提交
git log 查看所有commit的记录

## 开始git之旅
1. github上新建reposity，假设仓库名为Inbox
2. 本地化 
   - 配置 <br>
    git config --global user.name "yourname" <br>
    git config --global user.email "youremail" <br>
    <font color='red'>git config --global core.editor vim</font> <br>
    > 这一点很重要，可以摆出Git自带的愚蠢的GNO的困扰
    -  两种方式
       - git init <br>
        git remote add origin `http://website://repo.git` <br>
        > https://github.com/AugF/DesignPatterns.git
        git push -u origin master
       - git clone `http://website://repo.git`
## 多人开发思路：
git branch查看分支

git checkout -b fpm， 创建分支并转到分支
git checkout -d fpm,  删除分支

所有的修改操作都在fpm分支上
master负责merge远程分支

1. 希望master分支更新fpm分支上的内容
   - git checkout master
   - git merge fpm
   - git branch 查看分支
   - > fpm 分支并不会消失
2. 希望fpm更新到master分支上的内容
   - git checkout fpm
   - git rebase master
3. 希望能够显示本地分支的情况
4. merge前和push前需要检查本地内容有没有提交，请注意一件事就是先提交
5. 更新本地分支
    - git pull注意不过不带参数是直接覆盖
    - git pull origin next:master 取回origin主机的next分支与本地的master分支合并  !!!
        > 一种思路可以将代码全部拉到本地某个分支
        > git fetch origin + git merge origin/next
    - git branch --set-upstream master origin/next 自动为本地master追踪分支 
6. 远程分支操作
   - 查看远程分支： git branch -r
   - 创建运程分支： git push origin [name]
   - 删除远程分支： git push origin :[name]

 
## 对别人的项目做贡献
1. 非Collaborators
   - git remote add upstream 
   - git fetch upstream
   - git merge upstream/master

## 对远程仓库名进行查看

1. git remote show `<主机名>` : 查看远程主机，不包含地址
2. git remote add `<主机名>`: 添加远程主机
3. git remote rm `<主机名>`: 删除远程主机
4. git remote rename `<主机名>`: 改名
5. git remote -v: 连带查看地址
## 标签
```
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 删除本地tag
$ git tag -d [tag]

# 查看tag信息
$ git show [tag]
```

## 常见错误

1. You have not concluded your merge (MERGE_HEAD exists). Exiting because of unfinished merge
之前的提交没有merge
> git reset --merge

2. 无法推送一些引用到 'https://github.com/wangyunpan/nlp-zh-NER.git'
> git pull
> git push

3. 报错
fatal: 远程 origin 已经存在。
> git remote rm origin  删除远程分支

4. 如果我本地一顿猛操作，然后需要更新文件，然后应该怎么办？ 不能直接git pull, 再git push, 这样会覆盖本地的文件？？

5.  ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/AugF/DesignPatterns.git'
hint: Updates were rejected because the tip of your current branch is behind
    > git push origin master -f

6. OpenSSL: error:1409442E:SSL routines:ssl3_read_bytes:tlsv1 alert protocol version
    > 向pasa gitbucket提交出现该bug
    > 按照stackoverflow上  https://stackoverflow.com/questions/53151456/openssl-error1409442essl-routinesssl3-read-bytestlsv1-alert-protocol-versio
    > 修改  wget --secure-protocol=TLSv1_2 仍然未解决

7. 如何强制将远程代码覆盖到本地
    > git pull 并没有得到远程最新的代码
    > 使用 git fetch --all && git reset --hard origin/master && git pull 强制覆盖本地代码
    - git branch --set-upstream-to=origin/master branch
    - git pull 远程主机名  远程分支名:本地分支名

8. 如何将现有代码分支push到远程指定分支
    > git push --set-upstream origin current_branch

9. fatal: protocal 'https' is not supported
    > 1. use ssh
    > 2. 在git安装目录中找到libcurl-4.dll的文件位置，并移动到其他地方