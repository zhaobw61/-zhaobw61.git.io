---
layout:       post
title:        "git使用的基本技巧"
subtitle:     "常用的操作"
date:         2019-01-07 21:00:00
author:       "boowen"
header-img:   "img/post-bg-2018-11.jpg"
header-mask:  0.3
tags:
    - github
---
>整理文件的时候发现以前自己刚入门学习使用github的时候做得笔记，都是记录了一些基本用法。也算是自己的成长，就放在博客吧。

1.克隆一个项目：`git clone url`

2.提交文件：

添加要提交的文件: `git add. `

提交到缓存: `git commit -m '注释'`

推送到分支: `git push origin master`

>origin 是远端的仓库的名字，也可以其他的名字，最开始的创建默认的是origin

>matser 是指定提交到哪一个分支。


3.库的基本操作

查看库: `git remote -v`

删除库: `git remote rm name`

添加库: `git remote add name url`

4.分支的基本操作

查看分支: `git branch`

删除分支： `git branch -d name`

添加分支: `git checkout -b name`

 > 这个命令相当于，创建分支`git branch name`,然后切换分支`git checkout name`

切换分支: `git checkout name`


