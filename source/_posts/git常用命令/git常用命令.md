---
title: git常用命令
date: 2018-10-12 14:16:09
tags: GIT
categories: 前端
---
<!-- toc -->

### git 常用命令
#### 个人学习经历
　　从认识到使用git命令已有两年多的时间了，记得刚刚认识它的时候很是害怕，害怕那看不懂的命令，以至于每次提交代码都要用github客户端进行提交。当然了，那是的我对客户端也是晦涩难懂（现在也是），以至于要彻底放弃。有时每次提交代码都要把文件拉到网页上面进行提交，很是麻烦。后来跟着老师的课程渐渐地认识了git，才发现是如此的好用，慢慢的发现自己已经离不开它了。虽然以前已经整理过一些关于git的命令，但却凌乱不堪，以至于自己常常自惭形秽。今天我根据阮一峰老师的博客对自己的这篇文章进行重构，让自己的博客文章慢慢的有自己的思想和见解。
　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　--2019-3-8<!-- more -->

#### GIT命令
##### 1.新建代码库
```bash
# 在当前目录新建一个Git代码库
git init

# 新建一个目录，将其初始化为Git代码库
git init [project-name]

# 下载一个项目和它的整个代码历史
git clone [url]

# 例： git clone git@github.com:aLittleLittleStar/Travel.git
```

##### 2.配置
　　Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```bash
# 显示当前的Git配置
git config --list

# 编辑Git配置文件
git config -e [--global]

# 设置提交代码时的用户信息
git config [--global] user.name "[name]"
git config [--global] user.email "[email address]"
```

##### 3.增加/删除文件
```bash
# 添加指定文件到暂存区
git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
git add [dir]

# 添加当前目录的所有文件到暂存区
git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
git add -p

# 删除工作区文件，并且将这次删除放入暂存区
git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
git mv [file-original] [file-renamed]
```

##### 4.代码提交
```bash
# 提交暂存区到仓库区
git commit -m [message]

# 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

# 提交并添加注释
git commit -am "init"

# 提交时显示所有diff信息
git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
git commit --amend [file1] [file2] ...
```

##### 5.分支
```bash
# 查看本地所有分支，当前分支会被星号标示出
git branch

# 列出所有远程分支
git branch -r

# 列出所有本地分支和远程分支
git branch -a

# 可以看见每一个分支的最后一次提交
git branch -v

# 可以查看本地分支对应的远程分支
git branch -vv

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 新建一个分支，并切换到该分支
git checkout -b [branch]

# 新建一个分支，指向指定commit
git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
git checkout [branch-name]

#  切换到dev分支
git checkout dev

# 切换到上一个分支
git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
git merge [branch]

# 将分支dev与当前分支进行合并
git merge origin/dev

# 选择一个commit，合并进当前分支
git cherry-pick [commit]

# 给分支重命名
git branch -m oldName newName

# 删除分支，如果在分支中有一些未merge的提交，那么会删除分支失败
git branch -d [branch-name]

# 强制删除dev分支
git branch -D dev

# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```

##### 6.标签
```bash
# 列出所有tag
git tag

# 新建一个tag在当前commit
git tag [tag]

# 新建一个tag在指定commit
git tag [tag] [commit]

# 删除本地tag
git tag -d [tag]

# 删除远程tag
git push origin :refs/tags/[tagName]

# 查看tag信息
git show [tag]

# 提交指定tag
git push [remote] [tag]

# 提交所有tag
git push [remote] --tags

# 新建一个分支，指向某个tag
git checkout -b [branch] [tag]
```

##### 7.查看信息
```bash
# 显示有变更的文件
git status

#  查询repo的状态. -s表示short, -s的输出标记会有两列,
# 第一列是对staging区域而言,第二列是对working目录而言.
git status -s

# 显示当前分支的版本历史
git log

# 显示commit历史，以及每次commit发生变更的文件
git log --stat

# 搜索提交历史，根据关键词
git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
git log --follow [file]
git whatchanged [file]

# 显示指定文件相关的每一次diff
git log -p [file]

# 显示过去5次提交
git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
git blame [file]

# 显示暂存区和工作区的差异
git diff

# 显示暂存区和上一个commit的差异
git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
git diff HEAD

# 显示两次提交之间的差异
git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
git show [commit]

# 显示某次提交发生变化的文件
git show --name-only [commit]

# 显示某次提交时，某个文件的内容
git show [commit]:[filename]

# 显示当前分支的最近几次提交
git reflog
```

##### 8.远程同步
```bash
# 下载远程仓库的所有变动
git fetch [remote]

# 显示所有远程仓库
git remote -v

# 显示某个远程仓库的信息
git remote show [remote]

# 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

# 上传本地指定分支到远程仓库
git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

# 推送所有分支到远程仓库
git push [remote] --all
```

##### 9撤销
```bash
# git撤销本地所有未提交的更改
# 第一个命令只删除所有untracked的文件，如果文件已经被tracked,
# 修改过的文件不会被回退。而第二个命令把tracked的文件revert到
# 前一个版本，对于untracked的文件(比如编译的临时文件)都不会被删除。
git clean -df
git reset --hard


# 恢复暂存区的指定文件到工作区
git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
git stash
git stash pop
```

##### 10.其他
```bash
# 生成一个可供发布的压缩包
git archive

# 看你commit的日志
git log --oneline

# 查看帮助
git checkout --help

# 查看目录
ls

# 查看所有目录
ls -al

# 查看文件内容
cat git.md

# 查看git的版本信息
git --version
```

#### Git fetch && Git pull 详解
```bash
```

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



##### git fetch 更新远程代码到本地仓库
　　理解 **fetch** 的关键, 是理解 FETCH_HEAD，FETCH_HEAD指的是: 某个branch在服务器上的最新状态’。 这个列表保存在 .Git/FETCH_HEAD 文件中, 其中每一行对应于远程服务器的一个分支。
当前分支指向的FETCH_HEAD, 就是这个文件第一行对应的那个分支.
一般来说, 存在两种情况:
如果没有显式的指定远程分支, 则远程分支的master将作为默认的FETCH_HEAD.
如果指定了远程分支, 就将这个远程分支作为FETCH_HEAD.
__git fetch origin branch1__
这个操作是__git pull origin branch1__的第一步, 而对应的pull操作,并不会在本地创建新的branch。设定当前分支的 FETCH_HEAD' 为远程服务器的branch1分支`。 
 
这个命令可以用来测试远程主机的远程分支branch1是否存在, 如果存在, 返回0, 如果不存在, 返回128, 抛出一个异常.
__git fetch origin branch1:branch2__
首先执行上面的fetch操作，使用远程branch1分支在本地创建branch2(但不会切换到该分支),如果本地不存在branch2分支, 则会自动创建一个新的branch2分支,

如果本地存在branch2分支, 并且是`fast forward', 则自动合并两个分支, 否则, 会阻止以上操作.
fetch更新本地仓库两种方式：

``` js
//方法一
$ git fetch origin master //从远程的origin仓库的master分支下载代码到本地的origin master

$ git log -p master.. origin/master//比较本地的仓库和远程参考的区别

$ git merge origin/master//把远程下载下来的代码合并到本地仓库，远程的和本地的合并

//方法二
$ git fetch origin master:temp //从远程的origin仓库的master分支下载到本地并新建一个分支temp

$ git diff temp//比较master分支和temp分支的不同

$ git merge temp//合并temp分支到master分支

$ git branch -d temp//删除temp

```

__1、git reset __
没有push，这种情况发生在你的本地代码仓库,可能你add ,commit 以后发现代码有点问题.

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交commit_id(79f673d631b08907496ce792f429e1f00da25b73)，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard 79f673d631b08907496ce792f429e1f00da25b73。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
__2、git revert__
已经push，对于已经把代码push到线上仓库,你回退本地代码其实也想同时回退线上代码,回滚到某个指定的版本,线上,线下代码保持一致.你要用到下面的命令

git revert用一个新提交来消除一个历史提交所做的任何修改.

revert 之后你的本地代码会回滚到指定的历史版本,这时你再 git push 既可以把线上的代码更新.(这里不会像reset造成冲突的问题)

revert 使用,需要先找到你想回滚版本唯一的commit标识代码,可以用 git log 或者在adgit搭建的web环境历史提交记录里查看.

git revert c011eb3c20ba6fb38cc94fe5a8dda366a3990c61
__3、两者区别__

git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit看似达到的效果是一样的,其实完全不同.

第一:上面我们说的如果你已经push到线上代码库, reset 删除指定commit以后,你git push可能导致一大堆冲突（或git push -f强制推送）.但是revert 并不会.

第二:如果在日后现有分支和历史分支需要合并的时候,reset 恢复部分的代码依然会出现在历史分支里.但是revert 方向提交的commit 并不会出现在历史分支里.

第三:reset 是在正常的commit历史中,删除了指定的commit,这时 HEAD 是向后移动了,而 revert 是在正常的commit历史中再commit一次,只不过是反向提交,他的 HEAD 是一直向前的. 




<!-- ![git常用命令图片](/assets/images/git.png) -->
转载：
　　[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
参考文章: 
　　[git fetch 更新远程代码到本地仓库](https://www.cnblogs.com/chenlogin/p/6592228.html)
　　[Git 常用命令总结](https://blog.csdn.net/tomatozaitian/article/details/73515849)
　　[git常用命令大全](https://blog.csdn.net/halaoda/article/details/78661334)
　　[Git常用命令解说](https://blog.csdn.net/hangyuanbiyesheng/article/details/6731629)
　　[Git fetch & pull 详解](https://blog.csdn.net/qq_36113598/article/details/78906882)
> 