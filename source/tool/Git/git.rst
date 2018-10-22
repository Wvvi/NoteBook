.. contents::
   :depth: 3
..

简介
====

Git是目前世界上最先进的分布式版本控制系统。

工作原理
========

.. figure:: git工作原理.png
   :alt: git工作原理

   git工作原理

Workspace：工作区 Index / Stage：暂存区 Repository：仓库区（或本地仓库）
Remote：远程仓库

创建版本库
==========

什么是版本库？版本库又名仓库，英文名repository,你可以简单的理解一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改，删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻还可以将文件”还原”

::

    git init                       把当前目录变成git可以管理的仓库

    git add 文件名                  添加文件到暂存区

    git commit -m "你的注释"        把暂存区的文件提交当前分支上

    git status                     查看是否还有文件未提交

    git diff 文件名                 查看文件修改的内容

版本回退
========

::

    git log(git log –pretty=online)   查看历史记录

    git reset --hard HEAD^            把当前版本回退到上一个版本

    git reset --hard HEAD~100         把当前版本回退到前100个版本

    git reflog                        可以获得版本号

    git reset --hard 版本号            回退到指定的版本

撤销修改和删除文件操作
======================

::

    git checkout --文件名
    1、文件自动修改后，还没有放到暂存区，使用撤销修改就回到和版本库一模一样的状态
    2、文件已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态

    删除文件操作可以直接在文件目录中把文件删了，如果我想彻底从版本库中删掉了此文件的话，可以再执行commit命令提交。
    只要没有commit之前，如果我想在版本库中恢复此文件，可以使用git checkout --文件名

创建与合并分支
==============

::

    git checkout -b dev 表示创建并切换,相当于（git branch dev 和 git checkout dev）

    git branch          查看分支

    git branch name     创建分支

    git checkout name   切换分支

    git merge name      合并某分支到当前分支

    git branch -d name  删除分支

::

    分支管理策略：
    通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式

    创建一个dev分支。
    修改文件容。
    添加到暂存区。
    切换回主分支(master)。
    合并dev分支，使用命令 git merge –no-ff -m “注释” dev

多人协作
========

当你从远程库克隆时候，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且远程库的默认名称是origin。

要查看远程库的信息 使用 git remote 要查看远程库的详细信息 使用 git
remote –v

::

    推送分支就是把该分支上所有本地提交到远程库中，推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

    使用命令 git push origin master

多人协作时，大家都会往master分支上推送各自的修改

::

    首先要把dev也要推送到远程(git push origin dev)
    接着进入本地的目录，克隆远程库到本地(git clone)
    现在可以使用命令创建本地dev分支(git checkout  –b dev origin/dev)

    多人协作的工作模式：
    可以试图用git push origin branch-name推送自己的修改.
    如果推送失败，则因为远程分支比你的本地更新早，需要先用git pull试图合并。
    如果合并有冲突，则需要解决冲突，并在本地提交。再用git push origin branch-name推送
