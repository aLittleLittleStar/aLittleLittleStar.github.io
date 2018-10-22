---
title: git常用命令
date: 2018-10-12 14:16:09
tags:
  GIT
categories: 前端
---
<!-- toc -->

### git 常用命令
<!-- more -->
``` js
ls        // 查看目录
ls -al    //查看所有目录
cat git.md //查看文件内容
git --version   //查看git的版本信息
git clone git@github.com:aLittleLittleStar/Vue-Music-App.git  
//从服务器上将代码给拉下来
git init      //本地初始化

git branch    //查看本地所有分支，当前分支会被星号标示出
git branch -r //查看远程所有分支
git branch -a //查看所有分支
git branch -v //可以看见每一个分支的最后一次提交.
git branch dev                        //新建分支dev
git branch -d (branchname)            //删除一个分支, 如果在分支
                                      //中有一些未merge的提交，那么会删除分支失败
git branch -D dev                     //强制删除dev分支
git push origin --delete <BranchName> //删除远程分支
git branch -vv                        //可以查看本地分支对应的远程分支
git branch -m oldName newName         //给分支重命名

git status    //查看当前状态[组件、删除、修改了哪些]
git status -s // 查询repo的状态. -s表示short,
              // -s的输出标记会有两列,第一列是
              // 对staging区域而言,第二列是对working目录而言.

git commit    //提交
git commit -am "init"         //提交并添加注释

git checkout dev             // 切换到dev分支
git checkout -b dev          //建立一个新的本地分支dev并切换到该分支
git merge origin/dev         //将分支dev与当前分支进行合并
git checkout dev             //切换到本地dev分支
git log --online             //看你commit的日志


git checkout --help         // 查看帮助

```


#### Git fetch && Git pull 详解
__git fetch__是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。
__git pull__ 则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge，这样可能会产生冲突，需要手动解决。
__分支的概念：__
分支是用来标记特定代码的提交，每一个分支通过SHA1sum值来标识，所以对分支的操作是轻量级的，你改变的仅仅是SHA1sum值。

如下图所示，当前有2个分支，A,C,E属于master分支，而A,B，D,F属于dev分支
``` js
A----C----E（master）
 \
  B---D---F(dev)
```

它们的head指针分别指向E和F，对上述做如下操作：
``` js
git checkout master //选择or切换到master分支
git merge dev      //将dev分支合并到当前分支(master)中
```
之后的情形是这样的：
``` js
A---C---E---G(master)
 \         /
  B---D---F（dev）
```
现在A，B,C,D,E,F,G属于master，G是一次合并后的结果，是将E和Ｆ的代码合并后的结果，可能会出现冲突。而A,B，D,F依然属于dev分支。可以继续在dev的分支上进行开发:
``` js
A---C---E---G---H(master)
 \         /
  B---D---F---I（dev）
```
理解gitfetch,关键是理解FETCH_HEAD，FETCH_HEAD指的是：某个branch在服务器上的最新状态。

##### git fetch 用法
``` js
git fetch <远程主机名> //这个命令将某个远程主机的更新全部取回本地
```
如果只想取回特定分支的更新，可以指定分支名：
``` js
git fetch <远程主机名> <分支名> //注意之间有空格

```
最常见的命令如取回origin 主机的master 分支：
``` js
git fetch origin master
```
取回更新后，会返回一个FETCH_HEAD ，指的是某个branch在服务器上的最新状态，我们可以在本地通过它查看刚取回的更新信息：

``` js
 git log -p FETCH_HEAD
```

##### git pull 用法
前面提到，git pull 的过程可以理解为：
``` js
git fetch origin master //从远程主机的master分支拉取最新内容 
git merge FETCH_HEAD    //将拉取下来的最新内容合并到当前所在的分支中
```
即将远程主机的某个分支的更新取回，并与本地指定的分支合并，完整格式可表示为：
``` js
git pull <远程主机名> <远程分支名>:<本地分支名>
```
如果远程分支是与当前分支合并，则冒号后面的部分可以省略：
``` js
git pull origin next
```



<!-- ![git常用命令图片](/assets/images/git.png) -->

参考文章: [Git 常用命令总结](https://blog.csdn.net/tomatozaitian/article/details/73515849)、[git常用命令大全](https://blog.csdn.net/halaoda/article/details/78661334)、[Git fetch & pull 详解](https://blog.csdn.net/qq_36113598/article/details/78906882)
> 