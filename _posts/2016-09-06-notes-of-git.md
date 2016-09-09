---
date: 2016-09-06 18:13:31+00:00
layout: post
title: Git笔记
categories: 文档
tags: 笔记 
---

# Git笔记


|功能|命令|备注|
|----|----|----|
|初始化一个Git仓库|`git init`||
|**添加文件到Git仓库**|`git add <file>`||
||`git commit`||
|查看仓库当前状态|`git status`||
|查看修改具体内容|`git diff`||
|命令显示从最近到最远的提交日志[^commit id]|`git log`  `--pretty=oneline`|`--pretty=oneline`  单行显示|
|版本回退|`git reset` `HEAD^` `--hard`|`HEAD^`  上一个版本 `HEAD~100`  上100个版本   可用于撤销当前`git add`|
|查看历史命令|`git reflog`||
|丢弃工作区的修改（用版本库里的版本替换工作区的版本）|`git checkout -- file`||
|删除版本库的文件|`git rm`||
||||
|关联一个远程库|`git remote add origin git@server-name:path/repo-name.git`||
|第一次推送master分支的所有内容|`git push -u origin master`||
|推送最新修改|`git push origin master`||
|克隆仓库|`git clone git@server-name:path/repo-name.git`||
|创建分支|`git branch <name>`||
|切换分支|`git checkout <name>`||
|创建并切换分支|`git checkout -b <name>`|相当于  `git branche <name>` `git checkout <name>`|
|查看当前分支|`git branch`||
|合并指定分支到当前分支|`git merge <name>`||
|删除分支|`$ git branch -d <name>`||
|强行删除未被合并的分支|`git branch -D <name>`||
|普通模式合并|`--no-ff` `git merge --no-ff -m "<description>" <name>`|有分支，能看出做过合并  默认`fast forward`看不出做过合并|
|分支合并图|`git log --graph`||
|储藏当前工作现场|`git stash`||
|查看工作现场|`git stash list`||
|恢复stash内容|`git stash apply`||
|恢复内容并删除|`git stash pop`|prefer|
|查看远程库信息|`git remote`|`-v`查看详细|
|从远程抓取分支|`git pull`||
|在本地创建和远程分支对应的分支|`git checkout -b branch-name origin/branch-name`||
|建立本地分支和远程分支的关联|`git branch --set-upstream branch-name origin/branch-name`||
|打标签|`git tag <name>`|默认为HEAD，也可以指定一个commit id|
|查看所有标签|`git tag`||
|指定标签信息|`git tag -a <tagname> -m "blablabla.."`||
|用PGP签名标签|`git tag -s <tagname> -m "blablabla..."`||
|删除标签|`git tag -d `||
|推送某个标签到远程|`git push origin <tagname>`||
|推送全部尚未推送到远程的本地标签|`git push origin --tags`||
|远程删除标签|`git push origin :refs/tags/<tagname>`||
||||

[^commit id]:commit id(版本号)是一个SHA1计算出来的数字，用十六进制表示。

---
###工作区和暂存区

 - 目录（工作区）
     - .git文件夹（版本库）
         - stage/index（暂存区）
         - 分支

 
![工作区和暂存区](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0 "工作区和暂存区")

---
### 远程仓库
1. 创建SSH Key
`$ ssh-keygen -t rsa -C "youremail@example.com"`
2. 添加远程库 
    - 在GitHub创建一个仓库
Create a new repo
    - 远程库的名字就是origin
`$ git remote add origin git@github.com:pollyreal/learngit.git`
    - 把本地库所有内容推送到远程库
`$ git push -u origin master`
-u 关联本地的master分支和远程的master分支

3. 提交本地
    `$ git push origin master`

---
###GitHub
可以推送pull request给官方仓库来贡献代码。


