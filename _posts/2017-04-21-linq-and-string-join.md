---
date: 2017-04-21 14:58:31+00:00
layout: post
title: List中获得ID并转换成以逗号分隔的字符串
categories: 文档
tags: 问题 LINQ
---

Linq，String.Join
```csharp
var list = String.Join(",", c.Select(r => r.ID));
```

机智如我(*^__^*) 