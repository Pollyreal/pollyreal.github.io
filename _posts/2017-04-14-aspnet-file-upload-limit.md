---
date: 2017-04-14 11:15:31+00:00
layout: post
title: ASP.NET通过修改web.config修改上传文件限制
categories: 文档
tags: 笔记
---

```html
<httpRuntime maxRequestLength="31457280" executionTimeout="3000"/>
```
如果网站配置在IIS上，文件上传首先会经过IIS限制。