title: Git常用远程操作
s: how-to-use-git-remote-commands
date: 2012-01-03 16:29:29
tags:
---
#### Git常用远程操作

在对Git远程操作命令理解之前，首先看下图，该图可以直观地帮助我们理解各操作命令执行后所影响的区域。

> 本文假设你对git的常规概念如：工作区（workspace），索引区（index）,本地仓库（local repository）和远程仓库（remote repository）都已经有了初步的了解。

![Git](http://image.beekka.com/blog/2014/bg2014061202.jpg)
 
### git clone

该命令用于克隆一个远程仓库到本地文件。
> $ git clone https://github.com/bafeimao/pages1
> 该命令会在本地自动创建文件夹`pages1`

也可以指定要克隆到的本地文件夹名
> $ git clone https://github.com/bafeimao/pages1 blog
 
可以提供多种协议访问方式，例如，github上提供如下三种常见的协议：
> $ git clone https://github.com/bafeimao/pages1
> $ git clone git@github.com:bafeimao/pages1.git

### git remote

要想对远程仓库进行管理，首先要使用`remote add`命令添加一个远程仓库对应的名称（其他命令如：`git pull`,`git push`等操作都将依赖于该远程仓库名 ）

添加一个远程仓库(需要指定一个名称和仓库的具体地址)
> $ git remote add origin git@github.com:bafeimao/pages1.git

 查看当前已经添加过的所有远程仓库列表
> $ git remote
> origin

使用-v选项可以看到更具体的名称和远程仓库URL
> $ git remote -v
> origin	git@github.com:bafeimao/coconut-projects.git (fetch)
> origin	git@github.com:bafeimao/coconut-projects.git (push)

### git fetch

成功添加远程仓库后，一旦远程仓库有更新，我就可以使用`fetch`命令将远程更改取回到本地仓库， 注意：这里是将更改取到本地仓库（local repository）而非本地工作区(workspace)，详见参考图）
> $ git fetch origin

默认取回所有分支的更改，可以指定只获取具体分支的更改
> $ git fetch origin dev
 
### git pull
 
根据上图，也可以直接将远程的某个分支的更改取回到工作区（workspace）中的指定分支上，这就要用到`git pull`命令了，该命令的基本用法如下:

`$ git pull [<remote-repo> [<remote-branch>[:<local-branch>]]]`

将远程仓库master分支上的更改合并到本地工作区的dev分支
> $ git pull origin master:dev
> 
> 冒号后面的本地分支名可以省略，省略时表示合并到当前工作分支:
> $ git pull origin master

### git push

如果需要将本地仓库的更改提交到远程，就需要使用`git push`命令，该命令的基本用法如下：

`$ git push [<remote-repo> [<local-branch>[:<remote-branch>]]]`

将本地dev分支推送到远程dev分支
> $ git push origin dev:dev

另外可以设定当前工作分支（假设为dev）的upstream分支（即对应的远程分支），这样设定以后，在执行push时可以省略`本地分支名：远程分支名`，甚至远程仓库名称origin也可以省略
> $ git push --set-upstream origin dev

此时可以省略分支名
> $ git push origin

甚至连origin都可以省略
> $ git push

**注意**：如果指定的远程分支不存在，则会在远程自动创建 

如果没有指定本地分支名则表示删除远程分支，下面两种方式均执行删除远程分支dev
```
$ git push origin :dev 
$ git push origin --delete dev
```
