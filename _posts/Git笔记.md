---
layout: post
title: "Git个人笔记"
date: 2019.11.21
description: "Git学习总结"

tag: 博客 
---   
# Git是什么？
Git是目前世界上最先进的分布式版本控制系统（没有之一）。
### 那什么是版本控制器？
就是可以记录你任何时间的增删该查
### 创建版本库
什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
### 初始化git仓库
通过`git init`命令把这个目录变成Git可以管理的仓库，初始化Git仓库

版本控制器是没办法跟踪2进制文件的比如说是视屏或者是音频，不幸的是word格式也是2进制文件，所以编写的时候一定要使用纯文本。



# 添加文件到Git仓库

1.使用命令`git add  <file>`,注意，可反复多次使用，添加多个文件
2.使用命令`git commit -m <message>`,完成。


## Git命令
1.`1 file changed` 表示一个文件被改动
2.`2 insertions` 插入了两行内容
3.`git status` 命令可以时刻掌握仓库状态，查看文件是否修改过
4.`git diff` 可以查看修改过得内容
5.`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git `reset --hard commit_id`。
6.穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
7.要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本


## Git分区
Git分为三个区，分别是，工作区、暂存区、仓库区，工作区就是编辑文档的时候就是在工作区，暂存区就是使用git add添加之后的状态就是暂存区可以使用git diff查看 ，然后在使用git commit -m 提交之后的状态，也就是最终存储就是仓库区

## Git管理修改
Git跟踪管理的是修改，而不是文件。
在Git中修改文件，只要不add添加到暂存区，就不会被commit提交到仓库。

## Git撤销和修改
命令`git checkout -- `readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销修改，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。


## 删除文件
使用`rm`命令删除一个文件，如果一个文件被提交到版本库，那么你永远不用担心误删，但是只能回复到最新版本，你会丢失**最近一次提交修改的内容**。

如果一不小心删错了可以用`git checkout -- test.txt`命令来回复，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本:

## 添加远程仓库
把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
`$ git push origin master`现在可以用这个命令将最新的内容添加到远程终端上。
要关联一个远程库，使用命令`git remote add origin git@github.com:F-s-l/repo-name.git`；
关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

## Git克隆
要克隆一个仓库，首先必须知道仓库的地址，**比如**`https://github.com/F-s-l/gitskills`，然后使用`git clone`命令克隆。
`$ git clone git@github.com:F-s-l/gitskills.git`使用这个命令克隆远程仓库
Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

# Git分支管理
`git checkout <file>`这个命令可以切换分支。
`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。
`git merge` dev命令用于合并分支，指定分支合并到当前分支
`git branch -d <file>`用于删除指定分支
Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`


## Git分支管理策略
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。



## Bag分支
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动



开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D `强行删除。



## 多人协作

查看远程库信息，用`git remote`命令 。

查看远程库详细信息，用`git remote  -v`命令 。

推送分支，就是把该分支所有本地内容提交到远程库，推送时，要指定本地分支，这样，GIt就会把该分支推送到远程库对应的远程分支上

在多人协作开发时，假设当两个人在共同的一个分支上开发写项目，如果A在dev分支上修改了1.txt文档，添加并提交，然后到推送到远程仓库，恰巧B，也在dev分支下面修改了1.txt文档，这时候B添加并提交，然后在推送到远程库中就会发生冲突，推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：



- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

# 创建标签

- 命令`git tag `用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

- 命令`git tag -a  -m "blablabla..."`可以指定标签信息；

- 命令`git tag`可以查看所有标签。

### 操作标签

- 命令`git push origin `可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d `可以删除一个本地标签；
- 命令`git push origin :refs/tags/`可以删除一个远程标签。

# 自定义Git

比如，让Git显示颜色，会让命令输出看起来更醒目：git会适当的显示不同颜色
```
$ git config --global color.ui true
```







