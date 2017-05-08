---
date: 2017-05-08 9:41:31+00:00
layout: post
title: Angular2/4 表单问题
categories: 文档
tags: 问题 angular
---

在表单中需要注意的事项

1. 在ng2表单中使用ngModel需要注意,必须带有name属性或者使用 [ngModelOptions]=”{standalone: true}”，二选其一

2. 使用button时需要注明type类型,未注明类型的button会默认为submit，当你点击一个非提交表单按钮时也会提交表单，所以要注明type="button"