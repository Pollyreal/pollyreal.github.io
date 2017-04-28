---
date: 2017-04-28 20:10:31+00:00
layout: post
title: git将所有分支clone
categories: 文档
tags: 版本控制 git
---

```
git branch -a
```
列出所有分支名称如下： remotes/origin/dev remotes/origin/release
```
git checkout -b dev origin/dev
```
作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支

机智如我(*^__^*) 
