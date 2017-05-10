---
date: 2017-05-09 23:01:31+00:00
layout: post
title: Windows端口映射
categories: 文档
tags: 服务器 Windows
---

添加

`
netsh interface portproxy add v4tov4 listenaddress=监听地址 listenport=监听端口 connectaddress=映射地址 connectport=映射端口
`

查看
`
netsh interface portproxy show all
`

删除
`
netsh interface portproxy delete v4tov4 listenaddress=地址 listenport=端口
`
