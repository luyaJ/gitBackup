---
title: git学习笔记
toc: true
date: 2017-07-22 10:52:20
tags: git
---

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

<!--more-->

## 创建版本库(repository)

首先在合适的地方(我选择在github/Git/usr中)，创建一个空目录：

	$ mkdir learngit
	$ cd learngit
	$ pwd
	/usr/learngit

`pwd`命令用于显示当前目录

第二步，***初始化仓库***，在`Git/usr`中，输入命令 `git  init`, 这样瞬间Git就把仓库建好了，而且会告诉你是一个空的仓库，此时你会看到多了一个`.git`目录

如果你没有看到，那是因为这个目录被隐藏了，用` ls -ah `命令就可以看到。

![git init命令](http://ot4r4qnml.bkt.clouddn.com/gitinit.PNG)

## 把文件添加到版本库

先编写一个`readme.txt` 文件，内容如下

	Git is a version control system.
	Git is free software.

第一步，把文件添加到仓库

	$ git add readme.txt

第二步，把文件提交到仓库

	$ git commit -m "add readme.txt"

### 修改readme.txt文件

接着上面的内容，继续修改readme.txt文件，改成一下内容：

	Git is a distributed version control system.
	Git is free software.
	
掌握仓库当前的状态：

	$ git status

![](http://ot4r4qnml.bkt.clouddn.com/gitstatus.PNG)

上图告诉我们，readme.txt被修改过,但还没有准备提交的修改

如果说过了很久，你忘了自己修改了什么内容，那么使用下面的命令就可以知道了：

	 $ git diff "文件名"

接下来使用之前的 `git add readme.txt` ,  `git commit -m "add distributed"`

## 版本回退

1.查看修改的历史记录(显示是从近到远)：

	$ git log

![](http://ot4r4qnml.bkt.clouddn.com/gitlog.png)

也可以加上参数 `--pretty=oneline`让历史记录看起来更简单。

2.退回上一个版本：

	$ git reset --hard HEAD^

![](http://ot4r4qnml.bkt.clouddn.com/gitreset.png)

3.可是这个时候又想回到之前的版本怎么办，别急，只要你的命令行窗口没关掉，就往上找到`append GPL` 的 `commit id`是 `ee0a0cff...`(版本号只要前几位就行，git会自动查找)

	$ git reset --hard ee0a0cff

那要是命令行窗口关掉了呢？不怕，还是有办法的！Git提供了一个命令 `git reflog` 用来记录你的每一次命令。

## 删除文件

	$ git rm readme.txt

## 上传到github完整过程 --添加远程库

	echo "#DemoWebsite" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin https://github.com/luyaJ/DemoWebsite.git
	git push -u origin master

## 分支管理

### 创建`others`分支，并切换到`others`分支	

	$ git checkout -b others

相当于下面的命令：

	$ git branch others
	$ git checkout others

### 查看当前分支

	$ git branch

### 切换回`master`分支

	$ git checkout master

### 合并分支

	$ git merge others

### 删除分支

分支用完合并后就可以随意删除了：

	$ git branch -D others


## 总结

1. 创建空目录三步走：`mkdir 目录名字` ， `cd 目录名字` ， `pwd`
2. 初始化仓库：`git init`
3. 添加文件到仓库：`git add 文件名` 
4. 提交文件到仓库：`git commit -m “XXXX”`
(注意每次修改，如果不`add`到暂存区，那就不会加入到 `commit`中)

5. 掌握仓库当前的状态：`git status`
6. 查看文件修改内容： `git diff “文件名”`
7. 查看修改的历史记录: `git log`
8. 删除文件：`git rm 文件名`



最后附上[廖雪峰老师的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)



