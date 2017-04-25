---
date: 2017-04-25 14:08:31+00:00
layout: post
title: 惰性加载的版本问题
categories: 文档
tags: 问题 惰性加载
---

```
Uncaught TypeError: Cannot read property 'apply' of undefined
    at XMLHttpRequest.desc.get [as ontimeout] (zone.js:1265)
    at XHRLocalObject.AbstractXHRObject._cleanup (abstract-xhr.js:149)
    at XMLHttpRequest.xhr.onreadystatechange (abstract-xhr.js:125)
    at XMLHttpRequest.wrapFn (zone.js:1230)
    at ZoneDelegate.invokeTask (zone.js:398)
    at Zone.runTask (zone.js:165)
    at XMLHttpRequest.ZoneTask.invoke (zone.js:460)
```
```
npm install zone.js@0.8.5 --save
```

机智如我(*^__^*) 